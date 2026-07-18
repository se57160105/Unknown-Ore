--========================================================
-- UNKNOWN ORE
-- FOLDER STRUCTURE AND TASKS
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

RECOMMENDED STRUCTURE
---------------------

ReplicatedStorage
├── Config
│   ├── BalanceConfig.lua
│   ├── ItemConfig.lua
│   └── GameEnums.lua
│
└── Remotes
    ├── Mining
    ├── Storage
    ├── Fusion
    ├── Refinery
    ├── Analyze
    ├── Items
    ├── Shelf
    ├── Sell
    ├── Collection
    └── Progression

ServerScriptService
├── Data
│   ├── PlayerDataTemplate.lua
│   ├── PlayerDataService.lua
│   ├── ProfileValidator.lua
│   └── Migrations.lua
│
├── Services
│   ├── TransactionService.lua
│   ├── MiningService.lua
│   ├── StorageService.lua
│   ├── FusionService.lua
│   ├── RefineryService.lua
│   ├── AnalyzeService.lua
│   ├── ItemService.lua
│   ├── HeldItemService.lua
│   ├── ShelfService.lua
│   ├── BuffService.lua
│   ├── SellService.lua
│   ├── CollectionService.lua
│   ├── ProgressionService.lua
│   └── MonetizationService.lua
│
└── ServerMain.server.lua

StarterPlayer
└── StarterPlayerScripts
    └── Controllers
        ├── MiningController.lua
        ├── LabController.lua
        ├── AnalyzeController.lua
        ├── InventoryController.lua
        ├── HeldItemController.lua
        ├── ShelfController.lua
        ├── SellController.lua
        ├── CollectionController.lua
        └── ProgressionController.lua

TASK ORDER
----------
1. Resolve pending Progression schema decision. [RESOLVED UO-0003: persist
   upgrade levels/IDs, derive runtime from config; development save-data RESET
   approved so prototype legacy-conversion DQ-2..DQ-8 is off the critical path.
   See 08/10/13/17.]
2. Build GameEnums.
3. Build BalanceConfig.
4. Complete 25_ITEM_CATALOG_DESIGN decisions and approve stable ItemIds.
5. Build the complete 96-item ItemConfig from that authority.
6. Data foundation and migration.
7. TransactionService.
8. Progression + Storage.
9. Personal Mining Nodes.
10. Fusion + Refinery.
11. Analyze + Reveal.
12. Actual Items + Lock + Held Item.
13. Shelf + Buff.
14. Sell.
15. Collection.
16. Monetization.
17. 10-player Homebase/Social world.
18. UI production.
19. Balance simulation/playtest.
20. Tutorial.

DO NOT CREATE V1 TASKS FOR
--------------------------
Anomaly Ore
Trading / Exchange
Shelf Presets
Auto Equip Best
Rebirth
