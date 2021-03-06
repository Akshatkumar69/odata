YASP's data dumps are located on [Academic Torrents](http://academictorrents.com/collection/yasp-data-dumps).

We suggest streaming the file, decompressing and parsing the JSON as you go. For an example on how to do this (in Node.js of course), see [here](https://github.com/yasp-dota/yasp/blob/master/dev/export-example.js). This script dumps all the match_ids contained in the data file.

In general:
* Times are in seconds
* Positions are notated as:
```JS
{
    "X COORDINATE": {
        "Y COORDINATE": # of instances at this position
    }
}
```
```JS
[{ // Data dumps are arrays of match objects
    "match_id": 2000594819,
    "match_seq_num": 1764515493,
    "radiant_win": false,
    "start_time": 1450027278,
    "duration": 1089, // In seconds
    "tower_status_radiant": 1975, // Bitmask, see here https://wiki.teamfortress.com/wiki/WebAPI/GetMatchDetails#Tower_Status
    "tower_status_dire": 2047,
    "barracks_status_radiant": 63,
    "barracks_status_dire": 63,
    "cluster": 133,
    "first_blood_time": 203,
    "lobby_type": 1,
    "human_players": 10,
    "leagueid": 4176,
    "positive_votes": 0,
    "negative_votes": 0,
    "game_mode": 2,
    "engine": 1,
    "picks_bans": [{ // See https://wiki.teamfortress.com/wiki/WebAPI/GetMatchDetails#Result_data
        "is_pick": false,
        "hero_id": 69,
        "team": 0, // 0 or 1
        "order": 0
    }, ... ],
    "parse_status": 2,
    "chat": [{
        "time": -89,
        "type": "chat", // Player message or could be event type, I.E "CHAT_MESSAGE_TOWER_KILL"
        "unit": "MAKES ME HAPPY", // Name of Player
        "key": "go?", // What they said
        "slot": 2 // Their slot
    }, ... ],
    "radiant_gold_adv": [0, 0, 0, 0, 0, 0, 0, 0, 0, 33, -218, -318, -1064, -1633, -1563, -2536, -2760, -3556, -4784, -5523, -6289, -8281, -9691, -11233, -12322, -12840, -15617, -15778, -15778, -15778, -15778, -15778],
    "radiant_xp_adv": [0, 0, 0, 0, 0, 0, 0, 0, 0, 137, -96, -236, -846, -1211, -1282, -2153, -2317, -2999, -3917, -4722, -5590, -6596, -8472, -8760, -9853, -10754, -14250, -14470, -14470, -14470, -14470, -14470],
    "teamfights": [{ // Array of teamfight objects
        "start": 380,
        "end": 432,
        "last_death": 417,
        "deaths": 4,
        "players": [{ // Array of each player's team fight contribution, indexed by their slot.
            "deaths_pos": {
                "76": {
                    "162": 1
                }
            },
            "ability_uses": {
                "lion_impale": 1,
                "lion_voodoo": 1
            },
            "item_uses": {},
            "killed": {},
            "deaths": 1,
            "buybacks": 0,
            "damage": 31,
            "gold_delta": -89,
            "xp_delta": 113,
            "xp_start": 603,
            "xp_end": 716
        }, ... ]
    }, ... ],
    "version": 15, // Parse version, used internally for YASP
    "skill": 1, // Newer data dumps have this, the skill level (Normal, High, Very High)
    "players": [{ // Array of players
        "account_id": 223332951, // Standard WebAPI stuff
        "player_slot": 0,
        "hero_id": 26,
        "item_0": 46,
        "item_1": 36,
        "item_2": 38,
        "item_3": 0,
        "item_4": 0,
        "item_5": 188,
        "kills": 0,
        "deaths": 5,
        "assists": 1,
        "leaver_status": 0,
        "gold": 19,
        "last_hits": 3,
        "denies": 4,
        "gold_per_min": 110,
        "xp_per_min": 112,
        "gold_spent": 2085,
        "hero_damage": 1072,
        "tower_damage": 33,
        "hero_healing": 0,
        "level": 6,
        "ability_upgrades": [{
            "ability": 5044,
            "time": 774,
            "level": 1
        }, ... ],
        "additional_units": null,
        "stuns": 12.7967, // Total stun duration of all stuns by this user
        "max_hero_hit": {
            "type": "max_hero_hit",
            "time": 561,
            "max": true,
            "inflictor": "lion_impale",
            "unit": "npc_dota_hero_lion",
            "key": "npc_dota_hero_winter_wyvern",
            "value": 105,
            "slot": 0,
            "player_slot": 0
        },
        "times": [0, 60, 120, 180, 240, 300, 360, 420, 480, 540, 600, 660, 720, 780, 840, 900, 960, 1020, 1080, 1140, 1200, 1260, 1320, 1380],
        "gold_t": [0, 100, 200, 300, 400, 500, 600, 700, 846, 946, 1046, 1194, 1294, 1394, 1494, 1594, 1784, 1884, 1984, 2000, 2000, 2000, 2000, 2000],
        "lh_t": [0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3, 3, 3, 3],
        "xp_t": [0, 62, 135, 232, 304, 448, 510, 716, 849, 922, 1273, 1324, 1397, 1503, 1582, 1582, 1841, 1841, 2045, 2045, 2045, 2045, 2045, 2045],
        "obs_log": [{
            "time": 880,
            "key": [114, 92] // (x, y)
        }, {
            "time": 980,
            "key": [102, 116]
        }],
        "sen_log": [{
            "time": 183,
            "key": [114, 134]
        }],
        "purchase_log": [{
            "time": -85,
            "key": "courier"
        }, ... ],
        "kills_log": [{
            "time": 332,
            "key": "npc_dota_hero_ember_spirit"
        }, ... ],
        "buyback_log": [],
        "lane_pos": {
            "70": {
                "74": 2,
                "162": 2
            }, ...
        },
        "obs": {
            "102": {
                "116": 1 // Placed one observer at (102, 116)
            }
        },
        "sen": {
            "114": {
                "134": 1
            }
        },
        "actions": { // See https://github.com/yasp-dota/yasp/blob/master/json/order_types.json
            "1": 2588,
            "2": 140,
            "4": 247,
            "5": 10,
            "6": 17,
            "7": 4,
            "8": 4,
            "10": 58,
            "11": 6,
            "16": 23,
            "17": 1,
            "19": 3,
            "25": 4
        },
        "pings": {
            "0": 37 // Total number of pings
        },
        "purchase": {
            "courier": 1,
            "ward_sentry": 1,
            "branches": 3,
            "clarity": 1,
            "tango": 1,
            "ward_dispenser": 1,
            "tpscroll": 10,
            "ward_observer": 3,
            "smoke_of_deceit": 1,
            "circlet": 1,
            "magic_stick": 1,
            "magic_wand": 1
        },
        "gold_reasons": { // See https://github.com/yasp-dota/yasp/blob/master/json/gold_reasons.json
            "0": 715,
            "1": -565,
            "6": 25,
            "13": 94
        },
        "xp_reasons": { // See https://github.com/yasp-dota/yasp/blob/master/json/xp_reasons.json
            "0": 1,
            "2": 2045
        },
        "killed": {
            "npc_dota_creep_goodguys_melee": 4,
            "npc_dota_creep_badguys_ranged": 2,
            "npc_dota_tusk_frozen_sigil1": 1
        },
        "item_uses": {
            "courier": 1,
            "ward_sentry": 2,
            "tango": 4,
            "tpscroll": 6,
            "ward_observer": 2
        },
        "ability_uses": {
            "lion_voodoo": 7,
            "lion_impale": 9
        },
        "hero_hits": {
            "undefined": 13,
            "lion_impale": 7
        },
        "damage": {
            "npc_dota_creep_goodguys_melee": 482,
            "npc_dota_neutral_dark_troll": 52,
            "npc_dota_neutral_dark_troll_warlord": 54,
            "npc_dota_hero_ember_spirit": 352,
            "npc_dota_creep_badguys_melee": 1111
        },
        "damage_taken": {
            "npc_dota_neutral_dark_troll": 35,
            "npc_dota_neutral_dark_troll_warlord": 24,
            "npc_dota_hero_winter_wyvern": 1042,
            "npc_dota_creep_badguys_ranged": 109,
            "npc_dota_creep_badguys_melee": 234,
            "npc_dota_hero_ember_spirit": 682
        },
        "damage_inflictor": {
            "undefined": 479,
            "lion_impale": 690
        },
        "runes": {},
        "killed_by": {
            "npc_dota_hero_ember_spirit": 3,
            "npc_dota_hero_windrunner": 1,
            "npc_dota_hero_magnataur": 1
        },
        "modifier_applied": {
            "modifier_tango_heal": 4,
            "modifier_teleporting": 6,
            "modifier_lion_voodoo": 7,
            "modifier_lion_impale": 20
        },
        "kill_streaks": { // See https://github.com/yasp-dota/yasp/blob/master/json/kill_streaks.json
            "3": 1,
            "4": 1,
            "5": 1,
            "6": 1,
            "7": 1,
            "8": 1,
            "9": 1,
            "10": 1
        },
        "multi_kills": {
            "2": 2
        },
        "healing": {
            "npc_dota_hero_gyrocopter": 8
        }
    }, ... ]
```