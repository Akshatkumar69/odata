* The project uses a microservice architecture, in order to promote modularity and allow different pieces to scale on different machines.
* Build step.  `npm run build` executes the following.
    * `npm install` Downloads and installs the Node dependencies from npm.
    * `npm run maven` Uses Maven to build the Java parser.
    * `npm run webpack` Builds and minifies the client side JS using Webpack.
* Descriptions of each service:
    * `web`: An HTTP server which serves the web traffic.
        * All of the querying, filtering, aggregation, and caching happens here.
        * We use pm2 to be able to run multiple instances to serve our web traffic and reload the server when deploying new code (minimizes downtime due to rolling restart)
    * `retriever`: An HTTP server that accepts URL params `match_id` and `account_id`.
        * Interfaces with the Steam GC in order to return match details/profile card.
    * `workParser`: An HTTP client that requests work from `workServer`.
        * The server should send back a replay URL.
        * It expects a compressed replay file `.dem.bz2` at this location, which it downloads, streams through `bunzip2`, and then through the compiled parser.
        * The parser emits a newline-delimited JSON stream of events, which is picked up and combined into a monolithic JSON object.
        * This JSON is POSTed back to the server.
        * The schema for the current parsed_data structure can be found in `utility.getParseSchema`.
    * `workServer`: Handles clients looking for work.
        * Runs `buildSets` prior to start to ensure it has a retrievers list.
        * Listens for work requests from `workParser`.  When it gets one:
            * `getReplayUrl` to get the download URL for a replay.  It selects randomly from the list of available retrievers to serve the request.
            * Send the replay URL back to the client that requested it.
            * Listen for a POST from the parse worker and save the result to DB.
    * `worker`: Takes care of background tasks.  This is still a bit of a jack-of-all-trades since it used to process all job types before we moved some of them out to individual processes.
        * Processes incoming parse requests, using the `processApi` processor.
            * This could probably go in its own process.
        * Runs some functions on an interval.
            * `buildSets`.  Update sets of players based on DB/Redis state.
                * Rebuilds the sets of tracked players, donated players, and signed-in players by querying DB/Redis and saves them under keys in Redis.
                * It also creates Redis keys for parsers/retrievers by reading config. Could be extended later to query from a service discovery server.
            * Used to `updateNames`, which requested Steam personanames for 100 users on a timed interval.  We currently aren't updating names in the background.
            * Used to process API calls created by `updateNames`.
    * `scanner`: Reads the Steam sequential API to find the latest matches to add/parse.
        * Runs `buildSets` prior to start to ensure we have the latest trackedPlayers so we don't leak matches.
        * If a match is found passing the criteria for parse, `insertMatch` is called.
        * If `match.parse_status` is explicitly set to 0, the match is queued for parse.
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
* Parses come in one of two ways:
    * Sequential: We read a match from the Steam API that either has `leagueid>0` or contains a player in the `trackedPlayer` set.
    * Request: Requests are processed from the Request page.  This creates a job that reads the match data from the steam API, then waits for the parse to finish.
        * The client uses AJAX to poll the server.  When an error occurs or the job finishes, it either displays the error or redirects to the match page.
        * Requests are set to only try once.
* Player/match caching: 
    * We cache matches in Redis in order to reduce DB lookups on repeated loads.
    * Player caching is more complicated.  It means that whenever we add a match or add parsed data to a match, we need to update all of that match's player caches to reflect the change (to keep the cache valid).
* `npm run watch`: If you want to make changes to client side JS, you will want to run the watch script in order to automatically rebuild after making changes.
* `npm test` to run the full test suite.  Use `mocha` for more fine-grained control.
* Brief snippets and useful links are included in the [wiki](https://github.com/yasp-dota/yasp/wiki)