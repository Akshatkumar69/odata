* The project uses a microservice architecture, in order to promote modularity and allow services to scale individually and be deployed on a cluster of nodes.
* Build step.  `npm run build` executes the following.
    * `npm install` Downloads and installs the Node dependencies from npm.
    * `npm run maven` Uses Maven to build the Java parser.
    * `npm run webpack` Builds and minifies the client side JS using Webpack.
* Descriptions of each service:
    * `web`: An HTTP server which serves the web traffic.
        * All of the querying, filtering, aggregation, and caching happens here.
        * Views are also built by this service (currently using Jade as template engine).
        * Requests are processed from the Request page.  This creates a job that reads the match data from the steam API, then waits for the parse to finish.
            * The client uses AJAX to poll the server once the job is created.  When an error occurs or the job finishes, it either displays the error or redirects to the match page.
            * Requests are set to only try once.
    * `retriever`: An HTTP server that accepts URL params `match_id` and `account_id`.
        * Interfaces with the Steam GC in order to return match details/profile card.
    * `parser`: 
        * Invokes `getReplayUrl` to get the download URL for a replay.  It selects randomly from the list of available retrievers to serve the request.  The list of retriever hosts is defined in config.
        * The retriever should send back a replay URL.
        * The service expects a compressed replay file `.dem.bz2` at this location, which it downloads, streams through `bunzip2`, and then through the compiled Java parser.
        * The Java parser emits a newline-delimited JSON stream of events, which is picked up and combined into a monolithic JSON object.
        * The schema for the current parsed_data structure can be found in `utility.getParseSchema`.
    * `requests`: Handles incoming parse requests.
        * Requests basic match data from the API, then queues a parse job.  Waits for the result of the parse job.
    * `worker`: Takes care of background tasks by running functions on an interval.     
        * `buildSets`.  Update sets of players based on DB/Redis state.
            * Rebuilds the sets of tracked players, donated players, and signed-in players by querying DB/Redis and saves them under keys in Redis.
            * It also creates Redis keys for parsers/retrievers by reading config. Could be extended later to query from a service discovery server.
        * `getMmStats`.  Gets matchmaking queue stats from a retriever
        * `cleanup`.  Cleans up old jobs from the queue and old telemetry data.
    * `profiler`.  Selects a random block of 100 players from the database and requests new Steam player profile data for each.
    * `scanner`: Reads the Steam sequential API to find the latest matches to add/parse.
        * Runs `buildSets` prior to start to ensure we have the latest trackedPlayers so we don't leak matches.
        * If a match is found passing the criteria for parse (`leagueid>0` or contains player in `trackedPlayers` set), `insertMatch` is called.
        * If `match.parse_status` is explicitly set to 0, the match is queued for parse.
        * If the player is signed in and the match is ranked, queues MMR request.  Also queues MMR requests for random matches in order to update everyone's MMR periodically.
    * `proxy`: A standalone HTTP server that simply proxies all requests to the Steam API.
        * The host is functionally equivalent to `api.steampowered.com`.
    * `skill`: Reads the GetMatchHistory API in order to continuously find matches of a particular skill level.
        * Applying the following filters increases the number of matches we can get skill data for;
            * `min_players=10`
            * `hero_id=X`
            * By permuting all three skill levels with the list of heroes, we can get up to 500 matches for each combination.
    * `mmr`: Processes MMR requests.
        * Sends a request to the retriever asking for the player's MMR
    * `fullhistory`: Processes full history requests.
        * By querying for a player's most recent 500 matches (API limit) with each hero, get most/all of a player's matches.
    * `cacher`: Updates player caches when new matches are inserted.
* Player/match caching for performance:
    * We cache matches (JSON blobs) in Redis in order to reduce DB lookups on repeated loads.
    * We also cache player profiles.  The current caching model saves a JSON blob of the aggregated data.  This means:
        * The cache is invalid if there is a filter on the request.
        * Adding a match requires checking the cache for all the players within and updating them accordingly.
        * Currently the caches are stored as compressed JSON blobs, and updating requires a read followed by a full rewrite of the blob.  This is quite CPU-intensive.