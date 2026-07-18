--========================================================
-- UNKNOWN ORE
-- IMPLEMENTATION PLAN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

PHASE 0 — BASELINE
------------------
Freeze Latest Merged V1 Design.
Resolve Upgrade-Level persistence decision. [RESOLVED UO-0003: persist upgrade
levels/IDs; derive runtime from config. See 08/10/17.]
Create BalanceConfig.
Create GameEnums.
Complete the authoritative catalog design in 25_ITEM_CATALOG_DESIGN, approve stable
ItemIds, then create the production 96-item ItemConfig.

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

PHASE 7 — SHELF / BUFF
----------------------
Physical Shelves
Draft Editor
Real-time preview
Atomic Apply
Final-state Inventory validation
Visitor inspect

PHASE 8 — SELL / ECONOMY
------------------------
Sell Selected
Quantity controls
Quick Sell Common/Uncommon
Held/Locked/Shelf exclusions
Transaction-time Sell Buff
Telemetry

PHASE 9 — COLLECTION
--------------------
Personal Reveal discovery
Rarity sets
Manual Claim
24/24 Cave reward
96/96 Global reward

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

2. EXPAND (add, do not remove): after UO-3500 and approval of stable ItemIds,
   author GameEnums / BalanceConfig / ItemConfig + resolvers behind a temporary ADAPTER
   over the old config. A representative Crystal subset may prove mechanics, but it must
   reference approved catalog IDs; the temporary slot identifiers and nine legacy
   prototype identifiers in 25_ITEM_CATALOG_DESIGN are not silently final.
   Add new schema groups + compatibility accessors without deleting old fields.
   Add a buff-read interface (compat impl) so Mining/Sell read buffs before the
   Shelf rewrite - this breaks the Mining<->Buff cycle.

3. MIGRATE VERTICAL SLICES (each: server + UI + rollback; PROTOTYPE-conversion
   migration fixtures are NOT required per the UO-0003 reset - but each slice
   still ships the clean-schema forward path + rollback):
   Analyze FIRST (only after its result is redacted by the projection), then
   Items/Collection, then Storage/Deposit + Mining/Discovery/Progression, then
   Shelf/Buff + Sell.

4. ATOMIC SYSTEMS: Fusion + Refinery after Storage + replay-safety exist.

5. Then World/Social and Monetization (durable receipts), and FINALLY contract
   legacy fields + complete the full 96-item catalog + isolated hygiene cleanup
   (rename remotes, remove dead artifacts) after fleet convergence.

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
