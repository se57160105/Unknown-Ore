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
Runtime implementation of the authoritative identity contract in
25_ITEM_CATALOG_DESIGN. UO-3501 gives it exactly 96 immutable definitions with ItemId,
DisplayName, Cave, Rarity, and CollectionPosition. It owns no sell values, RewardPool
data, or visual fields. Gameplay systems consume it only after their later migrations.

Required dependency direction:
Game Design
-> Item Catalog Design (25_ITEM_CATALOG_DESIGN)
-> Runtime ItemConfig
-> Legacy Migration
-> Analyze Migration (including ContainerConfig reward references and Inventory grants)
-> Collection Migration
-> Shelf
-> Economy
-> UI

This order is mandatory. Catalog and ItemConfig work cannot be scheduled after any
Collection, Shelf, Economy, or UI work that depends on authoritative item identity.

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
