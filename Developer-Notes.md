Steam WebAPI documentation:
https://lab.xpaw.me/steam_api_documentation.html

Map:
https://github.com/kronusme/dota2-api/tree/master/images/map

Ability ID:
https://github.com/dotabuff/d2vpkr/blob/master/dota/scripts/npc/npc_abilities.json

Regions:
https://github.com/dotabuff/d2vpkr/blob/master/dota/scripts/regions.json

Code for rune spawns:

``` java
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

Unit orders (part of click events?):
```
enum dotaunitorder_t {
}
```

GameRulesState:
```
enum DOTA_GameState {
}
```

Game modes:
```
enum DOTA_GameMode {
}
```

Leaver status:
```
enum DOTALeaverStatus_t {
}
```

Combat log fields:
```
message CMsgDOTACombatLogEntry {
}
```