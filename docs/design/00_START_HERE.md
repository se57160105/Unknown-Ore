--========================================================
-- UNKNOWN ORE
-- 00 START HERE
-- AI CONTEXT ROUTER / DOCUMENT ENTRY POINT
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

PURPOSE
-------
This is the first Design Bible file that Claude or any AI coding assistant should read.

This file does not replace Project Overview.

Use:
00_START_HERE.lua
to decide which documents are required for the current task.

Use:
01_PROJECT_OVERVIEW.lua
to understand the game itself.

WORKFLOW FOR AI CODING ASSISTANTS
---------------------------------
STEP 1
Read this file first.

STEP 2
Identify the exact system or task being implemented.

STEP 3
Read only the REQUIRED documents listed for that system.

STEP 4
Inspect the current Roblox Studio project structure and existing code before writing or modifying code.

STEP 5
Check whether the current implementation conflicts with the Latest Merged Design Bible.

STEP 6
If there is a conflict:
- Do not silently choose the old implementation.
- Do not invent a replacement system.
- Report the conflict clearly.
- Follow the Latest Merged Design Bible unless the user explicitly changes the design.

STEP 7
Use:
BalanceConfig.lua
ItemConfig.lua
GameEnums.lua

as runtime sources of truth after those files are created.

For any task that defines, references, migrates, displays, discovers, shelves, or sells
collectible items, also read:
25_ITEM_CATALOG_DESIGN.lua

It is the authoritative design source for collectible identity and the only complete
96-slot catalog. Other documents must not maintain a competing item list.

For any task involving the nine retired legacy prototype Item identifiers (GemConfig's
Coal/Quartz/Ruby/Emerald/Sapphire/Diamond/AuroraCrystal/CelestialQuartz/SecretCrystalItem),
also read:
26_LEGACY_ITEM_DISPOSITION.lua

It is the authoritative source for retired legacy prototype Item dispositions - the single
per-identifier detail record. Other documents may summarize it but must not duplicate it.

WHEN TO READ EVERY DOCUMENT
---------------------------
Read the complete Design Bible only for:
- Full architecture review
- Cross-system design audit
- Major refactor
- Save schema redesign
- Pre-release consistency review

Do not reload every Design Bible document for every small implementation task.

GLOBAL REQUIRED RULES
---------------------
For every gameplay task, always follow:
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

For every profile mutation task, also read:
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua

SYSTEM CONTEXT ROUTER
=====================

PROJECT / FULL GAME UNDERSTANDING
---------------------------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
23_PROJECT_CHANGELOG.lua

OPTIONAL:
24_FUTURE_IDEAS.lua
99_ROADMAP.lua

MINING
------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
03_MINING_DESIGN.lua
08_PROGRESSION_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

STORAGE / DEPOSIT
-----------------
REQUIRED:
02_CORE_GAMEPLAY.lua
04_ANALYZE_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

FUSION
------
REQUIRED:
02_CORE_GAMEPLAY.lua
04_ANALYZE_DESIGN.lua
07_ECONOMY_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

REFINERY
--------
REQUIRED:
02_CORE_GAMEPLAY.lua
04_ANALYZE_DESIGN.lua
07_ECONOMY_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

ANALYZE / REVEAL
----------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
04_ANALYZE_DESIGN.lua
05_COLLECTION_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

ACTUAL ITEM / INVENTORY / LOCK
------------------------------
REQUIRED:
02_CORE_GAMEPLAY.lua
05_COLLECTION_DESIGN.lua
06_DISPLAY_SHELF_DESIGN.lua
07_ECONOMY_DESIGN.lua
09_UI_UX_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
18_CONTENT_DATABASE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

HELD ITEM
---------
REQUIRED:
02_CORE_GAMEPLAY.lua
06_DISPLAY_SHELF_DESIGN.lua
09_UI_UX_DESIGN.lua
10_DATA_STRUCTURE.lua
11_TECHNICAL_ARCHITECTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
18_CONTENT_DATABASE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

SHELF / BUFF
------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
06_DISPLAY_SHELF_DESIGN.lua
07_ECONOMY_DESIGN.lua
09_UI_UX_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
18_CONTENT_DATABASE.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

SELL / ECONOMY
--------------
REQUIRED:
02_CORE_GAMEPLAY.lua
06_DISPLAY_SHELF_DESIGN.lua
07_ECONOMY_DESIGN.lua
09_UI_UX_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

COLLECTION
----------
REQUIRED:
01_PROJECT_OVERVIEW.lua
05_COLLECTION_DESIGN.lua
09_UI_UX_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
18_CONTENT_DATABASE.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

PROGRESSION / PICKAXE / CAVE UNLOCK
-----------------------------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
03_MINING_DESIGN.lua
08_PROGRESSION_DESIGN.lua
10_DATA_STRUCTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

DATA / SAVE / MIGRATION
-----------------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
10_DATA_STRUCTURE.lua
11_TECHNICAL_ARCHITECTURE.lua
12_FOLDER_STRUCTURE_AND_TASKS.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua
23_PROJECT_CHANGELOG.lua

NETWORK / REMOTES / SECURITY
----------------------------
REQUIRED:
02_CORE_GAMEPLAY.lua
10_DATA_STRUCTURE.lua
11_TECHNICAL_ARCHITECTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

MONETIZATION / FINISH NOW
-------------------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
04_ANALYZE_DESIGN.lua
07_ECONOMY_DESIGN.lua
09_UI_UX_DESIGN.lua
10_DATA_STRUCTURE.lua
11_TECHNICAL_ARCHITECTURE.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua
24_FUTURE_IDEAS.lua

HOMEBASE / PLOT / SOCIAL
------------------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
09_UI_UX_DESIGN.lua
11_TECHNICAL_ARCHITECTURE.lua
13_IMPLEMENTATION_PLAN.lua
16_NETWORK_ARCHITECTURE.lua
20_CODING_RULES.lua
22_AI_DEVELOPMENT_RULES.lua

UI / UX
-------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
09_UI_UX_DESIGN.lua
Relevant system design file
16_NETWORK_ARCHITECTURE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

BALANCE REVIEW
--------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
03_MINING_DESIGN.lua
04_ANALYZE_DESIGN.lua
05_COLLECTION_DESIGN.lua
06_DISPLAY_SHELF_DESIGN.lua
07_ECONOMY_DESIGN.lua
08_PROGRESSION_DESIGN.lua
18_CONTENT_DATABASE.lua
19_BALANCING_GUIDE.lua
23_PROJECT_CHANGELOG.lua

FIRST PROTOTYPE
---------------
REQUIRED:
01_PROJECT_OVERVIEW.lua
02_CORE_GAMEPLAY.lua
03_MINING_DESIGN.lua
04_ANALYZE_DESIGN.lua
06_DISPLAY_SHELF_DESIGN.lua
07_ECONOMY_DESIGN.lua
10_DATA_STRUCTURE.lua
11_TECHNICAL_ARCHITECTURE.lua
12_FOLDER_STRUCTURE_AND_TASKS.lua
13_IMPLEMENTATION_PLAN.lua
14_FIRST_PROTOTYPE_SPEC.lua
15_SERVICE_RESPONSIBILITIES.lua
16_NETWORK_ARCHITECTURE.lua
17_SAVE_SYSTEM.lua
18_CONTENT_DATABASE.lua
19_BALANCING_GUIDE.lua
20_CODING_RULES.lua
21_NAMING_CONVENTION.lua
22_AI_DEVELOPMENT_RULES.lua

DOCUMENT ROLES
--------------
00_START_HERE.lua
AI context router and entry point.

01_PROJECT_OVERVIEW.lua
What Unknown Ore is.

02_CORE_GAMEPLAY.lua
End-to-end player loop and core pressure rules.

03–09
Gameplay-system design documents.

10–17
Data, architecture, service, network, and save contracts.

18–19
Content and balance sources.

20–22
Engineering and AI implementation rules.

23_PROJECT_CHANGELOG.lua
Latest locked changes and removed assumptions.

24_FUTURE_IDEAS.lua
Deferred and post-launch concepts.

25_ITEM_CATALOG_DESIGN.lua
Authoritative collectible identity, schema, and the 96-slot catalog.

26_LEGACY_ITEM_DISPOSITION.lua
Authoritative source for retired legacy prototype Item dispositions - the single detailed
per-identifier disposition record. Other documents may summarize it but must not duplicate it.

99_ROADMAP.lua
Development order.

CRITICAL CURRENT CORRECTIONS
----------------------------
Max Players per Server = 10.
Meteor Pickaxe Target = 6–8 active hours cumulative.
Meteor Relic Refine III = 10,000,000 Coins.

Do not use older values.

PROGRESSION PERSISTENCE - LOCKED (RESOLVED UO-0003, 2026-07-16)
--------------------------------------------------------------
[Was "PENDING TECHNICAL DECISION"; now LOCKED.]
DECISION: Save stable Upgrade Level/ID; derive capacities/counts/stats from
BalanceConfig. Derived values are never the authoritative saved state; premium
entitlements stay separate from coin progression. Resolves DQ-GLOBAL-1.
See 08_PROGRESSION_DESIGN, 10_DATA_STRUCTURE, 17_SAVE_SYSTEM.

LEGACY-DATA DECISIONS - RESOLVED/SUPERSEDED (UO-0003, 2026-07-16)
----------------------------------------------------------------
[Was "ADDITIONAL PENDING DECISIONS (AUDIT V2)".]
The DEVELOPMENT SAVE-DATA RESET is approved (17): prototype profiles are RESET,
not converted. Therefore DQ-2..DQ-8 (item identity/position, grade removal,
CaveDiscovery init, Collection mapping, in-flight analyze jobs, container
stacking, quarantine) are REMOVED from the current implementation critical path.
DQ-9 (provenance) remains OPEN and approval-gated (default NO). The clean V2
schema still begins its own forward-migration history; expand-migrate-contract
governs all FUTURE V2 schema changes. Catalog retained in 17_SAVE_SYSTEM as
historical reference.

ARCHITECTURE INTEGRATION (AUDIT V2 - APPROVED)
----------------------------------------------
The approved Architecture Audit V2 guidance is now MERGED INTO this Design
Bible. Implementation relies on the Bible ALONE; the separate audit documents
are historical and are not required reading. Where each concern now lives:
- Client replication = public projection/DTO; per-remote validation + abuse
  matrix; request/ack; receipt-callback ...... 16_NETWORK_ARCHITECTURE
- Expand-migrate-contract; session lease; DataStore observability; rollout/
  rollback; legacy migration decisions DQ-2..DQ-9 .... 17_SAVE_SYSTEM
- TransactionService applied SELECTIVELY (replay-safety by invariant, targeted
  serialization); Monetization durable receipt ledger .. 15_SERVICE_RESPONSIBILITIES
- Replay-safety/conservation invariants; projection rule; test gates ....
  20_CODING_RULES (rules 11-13)
- Analyze: persist + redact result; derived readiness; non-authoritative
  timers; no re-roll; Finish Now revalidation ...... 04_ANALYZE_DESIGN
- Item authorization = profile membership + Location; provenance approval-gated
  ................................................. 10_DATA_STRUCTURE
- Migration sequencing (interface-first; projection-before-persistence; config
  adapter; Crystal-subset-first) .................. 13_IMPLEMENTATION_PLAN
See 23_PROJECT_CHANGELOG for the integration record.
