Welcome to the yasp wiki!

Ability ID mappings:
https://github.com/dotabuff/d2vpk/blob/master/json/dota_pak01/scripts/npc/npc_abilities.json

Regions:
https://github.com/dotabuff/d2vpk/blob/master/json/dota_pak01/scripts/regions.json

Code for rune spawns:
```
Iterator<Entity> runes = ec.getAllByDtName("DT_DOTA_Item_Rune");
while (runes.hasNext()){
Entity e = runes.next();
Integer handle = e.getHandle();
if (!seenEntities.contains(handle)){
System.err.format("rune: time:%s,x:%s,y:%s,type:%s\n", time, e.getProperty("m_iRuneType"), e.getProperty("m_cellX"), e.getProperty("m_cellY"));
seenEntities.add(handle);
}
}
```

More detailed position data for entities:
https://gist.github.com/spheenik/3766744d47c170f25cf5.js

S1 Entities:
http://wiki.zynox.net/Network_Information

S2 Entities:
https://gist.github.com/howardchung/3f6873c8a942a20b7ab1

Unit orders (part of click events?):
```
enum dotaunitorder_t {
	DOTA_UNIT_ORDER_NONE = 0;
	DOTA_UNIT_ORDER_MOVE_TO_POSITION = 1;
	DOTA_UNIT_ORDER_MOVE_TO_TARGET = 2;
	DOTA_UNIT_ORDER_ATTACK_MOVE = 3;
	DOTA_UNIT_ORDER_ATTACK_TARGET = 4;
	DOTA_UNIT_ORDER_CAST_POSITION = 5;
	DOTA_UNIT_ORDER_CAST_TARGET = 6;
	DOTA_UNIT_ORDER_CAST_TARGET_TREE = 7;
	DOTA_UNIT_ORDER_CAST_NO_TARGET = 8;
	DOTA_UNIT_ORDER_CAST_TOGGLE = 9;
	DOTA_UNIT_ORDER_HOLD_POSITION = 10;
	DOTA_UNIT_ORDER_TRAIN_ABILITY = 11;
	DOTA_UNIT_ORDER_DROP_ITEM = 12;
	DOTA_UNIT_ORDER_GIVE_ITEM = 13;
	DOTA_UNIT_ORDER_PICKUP_ITEM = 14;
	DOTA_UNIT_ORDER_PICKUP_RUNE = 15;
	DOTA_UNIT_ORDER_PURCHASE_ITEM = 16;
	DOTA_UNIT_ORDER_SELL_ITEM = 17;
	DOTA_UNIT_ORDER_DISASSEMBLE_ITEM = 18;
	DOTA_UNIT_ORDER_MOVE_ITEM = 19;
	DOTA_UNIT_ORDER_CAST_TOGGLE_AUTO = 20;
	DOTA_UNIT_ORDER_STOP = 21;
	DOTA_UNIT_ORDER_TAUNT = 22;
	DOTA_UNIT_ORDER_BUYBACK = 23;
	DOTA_UNIT_ORDER_GLYPH = 24;
	DOTA_UNIT_ORDER_EJECT_ITEM_FROM_STASH = 25;
	DOTA_UNIT_ORDER_CAST_RUNE = 26;
	DOTA_UNIT_ORDER_PING_ABILITY = 27;
	DOTA_UNIT_ORDER_MOVE_TO_DIRECTION = 28;
}
```

GameRulesState:
```
enum DOTA_GameState {
	DOTA_GAMERULES_STATE_INIT = 0;
	DOTA_GAMERULES_STATE_WAIT_FOR_PLAYERS_TO_LOAD = 1;
	DOTA_GAMERULES_STATE_HERO_SELECTION = 2;
	DOTA_GAMERULES_STATE_STRATEGY_TIME = 3;
	DOTA_GAMERULES_STATE_PRE_GAME = 4;
	DOTA_GAMERULES_STATE_GAME_IN_PROGRESS = 5;
	DOTA_GAMERULES_STATE_POST_GAME = 6;
	DOTA_GAMERULES_STATE_DISCONNECT = 7;
	DOTA_GAMERULES_STATE_TEAM_SHOWCASE = 8;
	DOTA_GAMERULES_STATE_LAST = 9;
}
```

Game modes:
```
enum DOTA_GameMode {
	DOTA_GAMEMODE_NONE = 0;
	DOTA_GAMEMODE_AP = 1;
	DOTA_GAMEMODE_CM = 2;
	DOTA_GAMEMODE_RD = 3;
	DOTA_GAMEMODE_SD = 4;
	DOTA_GAMEMODE_AR = 5;
	DOTA_GAMEMODE_INTRO = 6;
	DOTA_GAMEMODE_HW = 7;
	DOTA_GAMEMODE_REVERSE_CM = 8;
	DOTA_GAMEMODE_XMAS = 9;
	DOTA_GAMEMODE_TUTORIAL = 10;
	DOTA_GAMEMODE_MO = 11;
	DOTA_GAMEMODE_LP = 12;
	DOTA_GAMEMODE_POOL1 = 13;
	DOTA_GAMEMODE_FH = 14;
	DOTA_GAMEMODE_CUSTOM = 15;
	DOTA_GAMEMODE_CD = 16;
	DOTA_GAMEMODE_BD = 17;
	DOTA_GAMEMODE_ABILITY_DRAFT = 18;
	DOTA_GAMEMODE_EVENT = 19;
	DOTA_GAMEMODE_ARDM = 20;
	DOTA_GAMEMODE_1V1MID = 21;
	DOTA_GAMEMODE_ALL_DRAFT = 22;
}
```

Leaver status:
```
enum DOTALeaverStatus_t {
	DOTA_LEAVER_NONE = 0;
	DOTA_LEAVER_DISCONNECTED = 1;
	DOTA_LEAVER_DISCONNECTED_TOO_LONG = 2;
	DOTA_LEAVER_ABANDONED = 3;
	DOTA_LEAVER_AFK = 4;
	DOTA_LEAVER_NEVER_CONNECTED = 5;
	DOTA_LEAVER_NEVER_CONNECTED_TOO_LONG = 6;
	DOTA_LEAVER_FAILED_TO_READY_UP = 7;
	DOTA_LEAVER_DECLINED = 8;
}
```

Combat log fields:
```
message CMsgDOTACombatLogEntry {
	optional DOTA_COMBATLOG_TYPES type = 1 [default = DOTA_COMBATLOG_DAMAGE];
	optional uint32 target_name = 2;
	optional uint32 target_source_name = 3;
	optional uint32 attacker_name = 4;
	optional uint32 damage_source_name = 5;
	optional uint32 inflictor_name = 6;
	optional bool is_attacker_illusion = 7;
	optional bool is_attacker_hero = 8;
	optional bool is_target_illusion = 9;
	optional bool is_target_hero = 10;
	optional bool is_visible_radiant = 11;
	optional bool is_visible_dire = 12;
	optional uint32 value = 13;
	optional int32 health = 14;
	optional float timestamp = 15;
	optional float stun_duration = 16;
	optional float slow_duration = 17;
	optional bool is_ability_toggle_on = 18;
	optional bool is_ability_toggle_off = 19;
	optional uint32 ability_level = 20;
	optional float location_x = 21;
	optional float location_y = 22;
	optional uint32 gold_reason = 23;
	optional float timestamp_raw = 24;
	optional float modifier_duration = 25;
	optional uint32 xp_reason = 26;
	optional uint32 last_hits = 27;
	optional uint32 attacker_team = 28;
	optional uint32 target_team = 29;
	optional uint32 obs_wards_placed = 30;
}
```