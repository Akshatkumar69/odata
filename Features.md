* Replay Parsing: Parses replays of Dota 2 matches to provide in-depth statistics for matches.
  * Item build times
  * Pick order
  * Number of pings
  * Stun/disable time
  * Consumables bought
  * Runes picked up
  * Laning position heatmap
  * Ward placement map
  * LHs per min table
  * Radiant advantage/Gold/XP/LH graphs per min
  * Teamfight summary
  * Objective times
  * Largest hit on a hero
  * Ability uses/hits
  * Item uses
  * Gold/XP breakdown
  * Damage/Kills crosstables
  * Multikills/Kill streaks
  * All chat
* Advanced Querying: Supports flexible querying and aggregation with the following criteria:
  * Player(s) in game (account ID)
  * Team composition (heroes)
  * Opponent composition (heroes)
  * Standard filters: patch, game mode, hero, etc.
* Aggregations:
  * Result count, win rate
  * Win rate by hour/day of week
  * Histograms (number of matches across Duration, LH, HD, TD, K, D, A, etc.)
  * Hero Matchups (win rate when playing as, with, against a hero)
  * Teammates/Opponents (win rate playing with/against particular players)
  * Max/N/Sum on multiple stat categories
  * Mean item build times
  * Skill accuracy
  * Records
  * Multikills/Kill Streaks
  * Laning
  * Ward Maps
  * Trends
  * Comparison against other users
  * Word Clouds (text said and read in all chat)
* Rating Tracker: Keep track of MMR by adding a Steam account as a friend
* Pro Games: Optionally parses professional matches: `leagueid>0`
* Modular: Microservice architecture, with pieces that can be used independently
* Scalable: Designed to scale to thousands of users.
* Free: No "premium" features.  All data is available for free to users.
* Open Source: All code is publicly available for feedback and contributions from the Dota 2 developer community.