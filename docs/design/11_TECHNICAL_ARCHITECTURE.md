--========================================================
-- UNKNOWN ORE
-- TECHNICAL ARCHITECTURE
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Keep all gameplay server-authoritative, modular, and safe for AI-assisted development.

CONFIG
------
BalanceConfig.lua:
Runtime source of truth for numerical balance.

ItemConfig.lua:
Runtime implementation of the authoritative identity and metadata contract in
25_ITEM_CATALOG_DESIGN. It owns the 96 Actual Item definitions; gameplay systems consume
it and may not become item-definition authorities.

Required dependency direction:
Design Item Catalog
-> Runtime ItemConfig
-> ContainerConfig reward references
-> Analyze
-> Inventory
-> Collection
-> Shelf
-> Economy
-> UI

GameEnums.lua:
Stable IDs/enums for Cave, Tier, Location, transaction/result states, etc.

SERVER SERVICES
---------------
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

DATA
----
PlayerDataTemplate
PlayerDataService
ProfileValidator
Migrations

CLIENT CONTROLLERS
------------------
MiningController
LabController
AnalyzeController
InventoryController
HeldItemController
ShelfController
SellController
CollectionController
ProgressionController
ShopController as needed

AUTHORITY
---------
Client requests intent.
Server validates and mutates.

Client never decides:
Drop Tier
ResultItemId
Sell payout
Coin balance
Item ownership
Shelf validity
Collection claim validity
Premium receipt grant

DERIVED VALUES
--------------
Use resolver/service functions.

Examples:
GetBackpackCapacity(profile)
GetLabCapacity(profile)
GetAnalyzeSlotCount(profile)
GetUnlockedShelfCount(profile)
GetActiveBuffs(profile)

Avoid scattered hard-coded values.

FEATURE SCOPE
-------------
Do not implement Anomaly Ore in V1.
Do not implement Trading in V1.

Future features must not leak partial schemas or unused receipt flows into Core MVP unless explicitly approved.
