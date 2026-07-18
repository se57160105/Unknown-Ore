--========================================================
-- UNKNOWN ORE
-- CONTENT DATABASE
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Keep content identity separate from balance logic and service code.

ITEM CATALOG AUTHORITY
----------------------
25_ITEM_CATALOG_DESIGN is the single authoritative design for collectible identity and
the only document containing the complete 96-slot Cave/Rarity/Position table.

Future runtime ItemConfig implements that design. ContainerConfig, Analyze, Inventory,
Collection, Shelf, Economy, and UI reference ItemIds and must not define a second catalog.

The current <CAVE>_<RARITY>_<POSITION> slot identifiers are temporary design identifiers
pending product approval. They must not be treated as approved final ItemIds. The nine
legacy prototype items and their unresolved mappings are documented only in
25_ITEM_CATALOG_DESIGN.

UNKNOWN CONTAINERS
------------------
Fragment = Common
Chunk = Uncommon
Cluster = Rare
Core = Epic
Heart = Legendary
Relic = Mythic

NORMAL POSITION DISTRIBUTION
----------------------------
A = 40%
B = 30%
C = 20%
D = 10%

REFINERY POSITION DISTRIBUTION
------------------------------
Normal: 40 / 30 / 20 / 10
I:      35 / 30 / 22 / 13
II:     30 / 30 / 24 / 16
III:    25 / 30 / 25 / 20

ITEM CONFIG METADATA
--------------------
The authoritative schema is defined in 25_ITEM_CATALOG_DESIGN:
ItemId, DisplayName, Cave, Rarity, CollectionPosition, Description, VisualKey, ModelKey,
IconKey, SellTier, and Tags. This document does not maintain a second schema or item list.

Gameplay Buff values should resolve from Cave identity + rarity scale + position modifier through config/resolvers.

Do not duplicate Buff numbers into every Item entry unless there is a deliberate override system.

CAVE BUFF IDENTITY
------------------
Crystal = Luck
Lava = Sell Price
Frozen = Analyze Speed
Meteor = Mining Speed

FUTURE CONTENT
--------------
Anomaly Ore is not V1 content.
Trading is not V1 content.
Do not add placeholder production items that imply these systems are active.
