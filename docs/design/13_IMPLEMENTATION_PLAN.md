--========================================================
-- UNKNOWN ORE
-- IMPLEMENTATION PLAN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

CANONICAL ITEM DEPENDENCY ORDER
-------------------------------
Game Design
-> Item Catalog Design
-> ItemConfig
-> Legacy Migration
-> Analyze Migration
-> Collection Migration
-> Shelf
-> Economy
-> UI

This order governs every item-identity implementation and migration step below. Existing
prototype runtime does not make a later consumer an authority or allow catalog work to be
scheduled after Collection.

PHASE 0 — BASELINE
------------------
Freeze Latest Merged V1 Design.
Resolve Upgrade-Level persistence decision. [RESOLVED UO-0003: persist upgrade
levels/IDs; derive runtime from config. See 08/10/17.]
Create BalanceConfig.
Create GameEnums.
Complete the authoritative catalog design in 25_ITEM_CATALOG_DESIGN, create the
production ItemConfig foundation using the locked ItemId format, and record the approved
retirement-without-mapping disposition before migrating Analyze or any downstream consumer.
Planning status: UO-3503 - Runtime Catalog Audit is completed. The future runtime migration
described in Phase 5 is a separate ticket whose number remains TBD; it is not created or
defined by this plan.

PHASE 1 — DATA FOUNDATION
-------------------------
PlayerDataTemplate
PlayerDataService
ProfileValidator
SchemaVersion
Migrations
TransactionService
RequestId/idempotency support

PHASE 2 — PROGRESSION AND STORAGE
---------------------------------
Pickaxe state
Cave unlocks
CaveDiscovery
Mining Backpack
Lab Storage
Capacity validation
Deposit flow

PHASE 3 — MINING MVP
--------------------
Personal node allocation
Spawn pools
Server mining state
Mining Speed snapshot
Luck completion read
Drop roll
Container grant
Backpack Full
Discovery Gate
Pickaxe/Cave flow

PHASE 4 — FUSION / REFINERY
---------------------------
Atomic batch Fusion
Refinery unlock
Refined Container Instances
Refinery pricing/distribution

PHASE 5 — ANALYZE
-----------------
Slots
Submit
Analyze Speed snapshot
Hidden ResultItemId
FinishAt/offline progress
Ready to Reveal
Manual Reveal
Inventory Full protection

PHASE 6 — ACTUAL ITEMS
----------------------
ItemUID
Inventory grouping
Lock
First Legendary/Mythic Auto Lock
Held Item model
Inspect

PHASE 7 — COLLECTION MIGRATION
------------------------------
Migrate discovery identity to ItemConfig after Analyze/Actual Item identity is authoritative.
Personal Reveal discovery
Rarity sets
Manual Claim
24/24 Cave reward
96/96 Global reward

PHASE 8 — SHELF / BUFF
----------------------
Consume ItemConfig-backed item metadata only after Collection migration.
Physical Shelves
Draft Editor
Real-time preview
Atomic Apply
Final-state Inventory validation
Visitor inspect

PHASE 9 — SELL / ECONOMY
------------------------
Consume ItemConfig or the explicitly referenced economy table after Shelf integration.
Sell Selected
Quantity controls
Quick Sell Common/Uncommon
Held/Locked/Shelf exclusions
Transaction-time Sell Buff
Telemetry

PHASE 10 — MONETIZATION
-----------------------
Premium Analyzer Slots
Developer Product receipt framework
Finish Now intent/buckets
Product metadata price
Receipt idempotency

PHASE 11 — WORLD / SOCIAL
-------------------------
10-player server layout
10 private plots
Central Hub
Cave access
Permissions
Visitor inspect
Travel-time validation

PHASE 12 — UI / UX
------------------
All production interfaces.

PHASE 13 — BALANCE PLAYTEST
---------------------------
6–8h Meteor Pickaxe target
Coin curve
Refinery affordability
Mythic frequency
Shelf power
Analyze pressure
Lab 100 pressure
Finish Now conversion
Expanded Lab candidate

PHASE 14 — TUTORIAL
-------------------
Design after physical loop and UI stabilize.


--========================================================
-- ARCHITECTURE ADDENDUM (AUDIT V2 - APPROVED, PRE-CODE)
--========================================================
MIGRATION SEQUENCING (existing Crystal prototype -> this design)
---------------------------------------------------------------
The phases above are the build order. Because a live Crystal prototype already
exists, apply them as a MIGRATION using EXPAND-MIGRATE-CONTRACT (17_SAVE_SYSTEM),
not a greenfield build. Order corrections remove dependency cycles by defining
narrow interfaces first:

0. FREEZE + DECIDE: [RESOLVED UO-0003, 2026-07-16] DQ-GLOBAL-1 resolved (persist
   upgrade levels/IDs; derive from config). DEVELOPMENT SAVE-DATA RESET approved
   (17): prototype profiles are RESET, not converted, so DQ-2..DQ-8 are REMOVED
   from the current critical path; DQ-9 (provenance) stays OPEN. Still define the
   invariants / test gates (rule 13 in 20_CODING_RULES) before coding.

1. BOUNDARIES FIRST (no gameplay schema change yet):
   a. Public projection / DTO (16) - MUST precede persisting any result.
   b. Session lease + DataStore observability (17).
   c. Version/migration contract + validator (the interface both migration and
      services depend on - this breaks the validator<->schema cycle).

2. ITEM CATALOG + ITEMCONFIG: DQ-IC-02 is resolved. UO-3501 authors the validated,
   read-only 96-entry ItemConfig using <Cave>_<Rarity>_<Position>. It does not add an
   adapter or migrate callers.

3. LEGACY DISPOSITION: DQ-IC-04 is resolved. All nine prototype identifiers are retired
   with no production mapping and must not enter ItemConfig. Runtime removal remains
   deferred to a later explicit migration ticket.

4. ANALYZE MIGRATION (future runtime migration ticket: TBD): migrate ContainerConfig
   reward references, Analyze result identity,
   and Inventory grants to authoritative ItemIds. Public projection/redaction must exist
   before persisted hidden results.

5. COLLECTION MIGRATION: migrate discovery keys and catalog presentation only after
   Analyze and Actual Item identity are authoritative.

6. SHELF, then 7. ECONOMY, then 8. UI: each consumes ItemConfig-backed identity and
   metadata. None may define or backfill catalog identity.

Unrelated Storage/Progression/Fusion/Refinery work follows its own safety dependencies,
but it cannot invert the item-identity order above. World/Social and Monetization follow
their existing contracts. Final names/assets and isolated hygiene remain approval-gated
content work; they do not move Item Catalog authority after downstream consumers.

Config migrates behind the adapter caller-by-caller; no atomic hard cutover.
Every schema-touching step ships with the deployment / rollback gates in 17.

UO-0003 RESOLUTION SUMMARY (2026-07-16)
---------------------------------------
Blocking decisions resolved: (1) development save-data RESET approved (17); (2)
progression persists upgrade levels/IDs and derives runtime from config
(08/10/17). Effect on critical path: prototype legacy-conversion work DQ-2..DQ-8
is REMOVED. STILL REQUIRED and unchanged: config foundation (GameEnums /
BalanceConfig / ItemConfig), the public projection / DTO, persistence safety
(session lease + observability), and the clean V2 schema + validator.
Expand-migrate-contract still governs all FUTURE V2 schema changes (not the
one-time prototype reset). DQ-9 (provenance) remains OPEN.
