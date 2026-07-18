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

ITEM ID CANDIDATE
-----------------
<CAVE>_<RARITY>_<POSITION>

Example design slot:
METEOR_MYTHIC_D

This is the temporary 96-slot design identifier pattern in 25_ITEM_CATALOG_DESIGN, not an
approved final ItemId convention. Final stable ItemIds require product approval. No
runtime or design document may silently treat the candidate pattern as final.

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
