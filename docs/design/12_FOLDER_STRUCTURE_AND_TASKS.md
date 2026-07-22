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

ITEM IDENTITY DEPENDENCY ORDER
------------------------------
Game Design
-> Item Catalog Design
-> ItemConfig
-> Legacy Migration
-> Analyze Migration
-> Collection Migration
-> Shelf
-> Economy
-> UI

TASK ORDER
----------
1. Resolve pending Progression schema decision. [RESOLVED UO-0003: persist
   upgrade levels/IDs, derive runtime from config; development save-data RESET
   approved so prototype legacy-conversion DQ-2..DQ-8 is off the critical path.
   See 08/10/13/17.]
2. Build GameEnums.
3. Build BalanceConfig.
4. Record the approved DQ-IC decisions in 25_ITEM_CATALOG_DESIGN.
5. Build the validated, read-only 96-entry ItemConfig using the locked ItemId format.
6. Data foundation and TransactionService.
7. Progression + Storage.
8. Personal Mining Nodes.
9. Fusion + Refinery.
10. Record the approved retirement-without-mapping legacy disposition; defer runtime removal.
11. Migrate Analyze reward/result identity and Actual Item grants.
12. Migrate Collection discovery and presentation identity.
13. Integrate Shelf + Buff with ItemConfig-backed metadata.
14. Integrate Sell/Economy with authoritative metadata.
15. Complete UI against authoritative replicated data.
16. Monetization.
17. 10-player Homebase/Social world.
18. Balance simulation/playtest.
19. Tutorial.

No downstream item consumer may define catalog identity or move ahead of its dependency
in the item-identity order above.

DO NOT CREATE V1 TASKS FOR
--------------------------
Anomaly Ore
Trading / Exchange
Shelf Presets
Auto Equip Best
Rebirth
