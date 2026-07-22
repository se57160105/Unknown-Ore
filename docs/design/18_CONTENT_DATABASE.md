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

UO-3501 ItemConfig implements that design as an isolated runtime foundation.
ContainerConfig, Analyze, Inventory, Collection, Shelf, Economy, and UI are not migrated
by UO-3501 and must not define a second catalog.

The approved production format is <Cave>_<Rarity>_<Position>. The nine legacy prototype
identifiers are retired with no production mapping. 26_LEGACY_ITEM_DISPOSITION is the
authoritative detailed per-identifier disposition record (see UO-3502); other documents may
contain short summaries or references to it, but no document may maintain a duplicate
detailed disposition table.

PROTOTYPE RUNTIME - NOT PRODUCTION DESIGN:
Current Crystal-only content, legacy identifiers, partial CollectionCatalog, placeholder
visuals, random Quality/Value behavior, prototype reward pools, and the absence of runtime
ItemConfig are documented in 25_ITEM_CATALOG_DESIGN. They are compatibility facts, not a
second catalog or production requirements.

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
The UO-3501 schema is defined in 25_ITEM_CATALOG_DESIGN and contains exactly:
ItemId, DisplayName, Cave, Rarity, CollectionPosition.

Names and visual references are deferred to UO-3600. Sell values derive from Rarity
through centralized Economy configuration. RewardPool composition/weights belong to
RewardPoolConfig. This document does not maintain a second schema or item list.

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
