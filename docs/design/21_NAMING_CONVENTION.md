--========================================================
-- UNKNOWN ORE
-- NAMING CONVENTION
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

CAVE IDS
--------
CRYSTAL
LAVA
FROZEN
METEOR

RARITY IDS
----------
COMMON
UNCOMMON
RARE
EPIC
LEGENDARY
MYTHIC

CONTAINER DISPLAY NAMES
-----------------------
Fragment
Chunk
Cluster
Core
Heart
Relic

PRODUCTION ITEM ID - LOCKED (DQ-IC-02 RESOLVED)
------------------------------------------------
<Cave>_<Rarity>_<Position>

Examples:
Crystal_Common_A
Lava_Rare_C
Frozen_Legendary_B
Meteor_Mythic_D

Use only the approved Cave and Rarity spelling shown above and position A/B/C/D.
DisplayName is never ItemId. Production ItemIds are stable and must not be renamed after
release.

CONTAINER STACK KEY
-------------------
<CAVE>:<TIER>

Example:
METEOR:EPIC

INSTANCE IDS
------------
ItemUID
ContainerUID
JobId
RequestId

LOCATIONS
---------
INVENTORY
SHELF

Do not add HELD as a durable Item Location.
HeldItemUID is Runtime State.

SERVICE NAMES
-------------
TransactionService
MiningService
StorageService
FusionService
RefineryService
AnalyzeService
ItemService
HeldItemService
ShelfService
BuffService
SellService
CollectionService
ProgressionService
MonetizationService

CONTROLLER NAMES
----------------
MiningController
LabController
AnalyzeController
InventoryController
HeldItemController
ShelfController
SellController
CollectionController
ProgressionController

REMOTE INTENT STYLE
-------------------
RequestAnalyzeSubmit
RequestReveal
RequestShelfApply
RequestSellSelected
RequestQuickSell

Prefer action intent names.
Do not name a client Remote as if the client grants a reward.
Bad:
GrantOre
GiveCoins
CompleteAnalyzeReward
