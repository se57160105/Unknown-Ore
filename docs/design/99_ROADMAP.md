--========================================================
-- UNKNOWN ORE
-- DEVELOPMENT ROADMAP
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

PHASE 0 — BASELINE LOCK
-----------------------
Freeze Latest Merged V1 Design.
Resolve pending Progression schema decision.
  [RESOLVED UO-0003, 2026-07-16] Progression persists upgrade levels/IDs and
  derives runtime values from config. Development save-data RESET approved, so
  prototype legacy-conversion DQ-2..DQ-8 is OFF the current critical path.
  Still required: config foundation, public projection, persistence safety
  (session lease + observability), and the clean V2 schema + validator.
  Expand-migrate-contract still applies to future V2 schema changes. DQ-9
  (provenance) remains open. See 13_IMPLEMENTATION_PLAN and 17_SAVE_SYSTEM.
Create BalanceConfig.
Create GameEnums.
Complete UO-3500 authoritative Item Catalog design and approve stable ItemIds before the
production ItemConfig and downstream content migrations.

PHASE 1 — DATA FOUNDATION
-------------------------
PlayerDataTemplate
PlayerDataService
ProfileValidator
SchemaVersion
Migrations
TransactionService
RequestId/idempotency
Service startup

PHASE 2 — PROGRESSION AND STORAGE
---------------------------------
ProgressionService
Pickaxe state
Cave unlock state
CaveDiscovery
Mining Backpack
Lab Storage
Capacity validation
Deposit

PHASE 3 — MINING MVP
--------------------
Personal nodes
Spawn pool
Server mining state
Mining Speed snapshot
Luck completion read
Drop roll
Container grant
Backpack Full
Discovery Gate
Cave unlock flow

PHASE 4 — FUSION AND REFINERY
-----------------------------
FusionService
Atomic batch Fusion
Refinery unlock
Refined Container Instances
Refinery prices
Position distributions

PHASE 5 — ANALYZE
-----------------
AnalyzeService
Slot progression
Submit
Analyze Speed snapshot
Hidden ResultItemId
FinishAt/offline progress
Ready to Reveal
Manual Reveal
Full Inventory protection

PHASE 6 — ACTUAL ITEMS
----------------------
ItemService
ItemUID
Inventory grouping
Lock
First Legendary/Mythic Auto Lock
Inspect
HeldItemService
HeldItemController
Attachment/Weld display

PHASE 7 — SHELF AND BUFF
------------------------
ShelfService
BuffService
Physical Shelves
Draft Editor
Real-time Preview
Atomic Apply
Final-state Inventory capacity validation
Visitor inspect

PHASE 8 — SELL AND ECONOMY
--------------------------
SellService
Sell Selected
Quantity controls
Quick Sell Common
Quick Sell Uncommon
Locked/Held/Shelf exclusions
Transaction-time Sell Buff
Economy telemetry

PHASE 8.5 — AUTHORITATIVE CONTENT DATABASE
------------------------------------------
Complete before further Collection UI completion.

Conceptual backlog sequence (roadmap only; these are not implementation tickets created
by this design task):
UO-3500 - Authoritative Item Catalog Design Refactor
UO-3501 - Runtime ItemConfig Foundation
UO-3502 - Legacy Item Mapping Decision
UO-3503 - Container and Analyze Migration
UO-3504 - Collection Migration

Final stable ItemIds and legacy mappings must be approved before downstream production
content migration. See 25_ITEM_CATALOG_DESIGN.

PHASE 9 — COLLECTION
--------------------
Personal Reveal discovery
Rarity sets
Manual Claim
24/24 rewards
96/96 reward
Trophy/Plot Effect hooks

PHASE 10 — MONETIZATION
-----------------------
Extra Analyzer Slot I
Extra Analyzer Slot II
Developer Product receipt framework
Finish Now purchase intent
Remaining-time bucket mapping
Product metadata price display
Receipt idempotency

PHASE 11 — HOMEBASE AND SOCIAL
------------------------------
10-player server layout
10 private plots
Ownership permissions
Central Hub
Cave access
Visitor access
Shelf/Trophy inspect
Travel-time validation

PHASE 12 — UI / UX PRODUCTION
-----------------------------
Mining HUD
Backpack
Lab Storage
Fusion
Refinery
Analyze
Reveal
Inventory
Held Item
Shelf Editor
Sell
Collection
Progression
Shop

PHASE 13 — BALANCE SIMULATION / PLAYTEST
----------------------------------------
Validate 6–8h Meteor Pickaxe target.
Coin source/sink simulation.
Refinery affordability.
Mythic acquisition frequency.
Shelf power curve.
Analyze Slot pressure.
Lab 100 pressure.
Finish Now bucket conversion.
Evaluate Expanded Lab +100.

PHASE 14 — TUTORIAL
-------------------
Design after physical loop and UI are stable.

POST-LAUNCH CANDIDATES
----------------------
Anomaly Ore
Specimen Exchange / Trading
Expanded Lab Storage if playtest supports it
Additional social systems
