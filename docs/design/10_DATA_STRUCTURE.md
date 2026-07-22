--========================================================
-- UNKNOWN ORE
-- DATA STRUCTURE DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Support progression, stacked containers, unique refined containers, unique Actual Items, Analyze Jobs, Shelf references, Collection history, and future migration.

PROFILE ROOT
------------
PlayerProfile
├── SchemaVersion
├── Meta
├── Economy
├── Progression
├── CaveDiscovery
├── Storage
├── Analyze
├── Items
├── Shelf
├── Collection
├── Entitlements
├── Settings
└── Statistics

UNREFINED CONTAINERS
--------------------
Stack by:
CaveId:TierId

Example:
Storage.Lab.Unrefined["METEOR:EPIC"] = 12

REFINED CONTAINERS
------------------
Unique Instances.

Recommended:
{
    ContainerUID = "...",
    CaveId = "METEOR",
    TierId = "MYTHIC",
    RefinementLevel = 3,
    CreatedAt = 0,
}

ACTUAL ITEMS
------------
Unique Instances.

Recommended:
{
    ItemUID = "...",
    ItemId = "...",
    OwnerUserId = 0,
    OriginUserId = 0,
    OriginAnalyzeJobId = "...",
    ObtainedAt = 0,
    Location = "INVENTORY",
    IsLocked = false,
}

Same ItemId has identical gameplay stats.

No random stat roll.
No Item Grade.

ITEM CATALOG IDENTITY CONTRACT
------------------------------
Each production Actual Item instance stores a stable ItemId reference to
25_ITEM_CATALOG_DESIGN and ItemConfig. ItemConfig owns DisplayName, Cave, Rarity, and
CollectionPosition. Names/visuals remain deferred; sell value is derived from ItemConfig
Rarity through centralized Economy configuration.

Collection discovery is keyed by stable ItemId. Catalog entries are content, not
per-player save records. The nine prototype identifiers are retired with no production
mapping. UO-3501 changes no profile schema and migrates no saved data.

ANALYZE JOB
-----------
Recommended:
{
    JobId = "...",
    CaveId = "...",
    TierId = "...",
    RefinementLevel = 0,
    StartedAt = 0,
    FinishAt = 0,
    ResultItemId = "...",
}

Do not save RemainingSeconds.

SHELF
-----
Shelf stores ItemUID references.

Example:
Shelf.Slots["SHELF_1:1"] = ItemUID

BUFFS
-----
Do not save derived active Buff totals.

Calculate from current Shelf mapping + ItemConfig + BalanceConfig.

CAVE DISCOVERY
--------------
Separate from Collection.

Stores personal mining tier discoveries.

Never infer CaveDiscovery from owned Actual Items.

COLLECTION
----------
Historical personal Analyze Reveal discovery.

Do not infer from current Inventory.

ENTITLEMENTS
------------
Separate premium ownership from Coin progression.

Examples:
PremiumAnalyzerSlotI
PremiumAnalyzerSlotII

RUNTIME ONLY
------------
Personal node runtime state
HeldItemUID
Derived Buff totals
Remaining Analyze seconds

SCHEMA VERSION
--------------
Use SchemaVersion and Migrations.lua.

PROGRESSION PERSISTENCE - LOCKED (RESOLVED UO-0003, 2026-07-16)
-------------------------------------------------------------
[Was "PENDING TECHNICAL DECISION"; now LOCKED per UO-0003.]
DECISION: Persist Upgrade Level / ID; derive capacity/count values from config.

Recommended Progression fields:
CurrentPickaxeId
HighestCaveUnlocked
BackpackUpgradeLevel
AnalyzeSlotUpgradeLevel
ShelfUnlockLevel
RefineryUnlocked

Then resolve:
Backpack Capacity
Coin Analyze Slots
Unlocked Shelf Count

from BalanceConfig.

[RESOLVED UO-0003, 2026-07-16] LOCKED (no longer pending): persist stable
upgrade levels/IDs; derive runtime capacities/counts/stats from BalanceConfig
via stable resolvers; server authoritative; premium entitlements kept separate.
See 08_PROGRESSION_DESIGN and 17_SAVE_SYSTEM. Resolves DQ-GLOBAL-1.


--========================================================
-- ARCHITECTURE ADDENDUM (AUDIT V2 - APPROVED, PRE-CODE)
--========================================================

ITEM AUTHORIZATION
------------------
In V1, item ownership/authorization is server-side PROFILE MEMBERSHIP plus the
item's Location/state - NOT a stored owner field. The server resolves the
acting player from the Remote sender (see 16_NETWORK_ARCHITECTURE).
ItemUID (unique instance id) IS required now (shelves, unique instances).
OwnerUserId / OriginUserId / OriginAnalyzeJobId are FUTURE provenance; adding
them in V1 is APPROVAL-GATED (DQ-9 in 17_SAVE_SYSTEM; see 24_FUTURE_IDEAS). A
duplicated owner field can go stale and is not an authorization boundary.

SERVER-ONLY FIELDS ARE REDACTED FROM REPLICATION
------------------------------------------------
Persisting a server-only field (e.g. a locked analyze ResultItemId) does NOT
make it client-visible: clients receive only the public projection with such
fields redacted (see 16_NETWORK_ARCHITECTURE and 04_ANALYZE_DESIGN).

LEGACY MIGRATION DECISIONS  [UPDATED UO-0003, 2026-07-16]
--------------------------------------------------------
The legacy schema conversions (item identity/position, grade removal,
CaveDiscovery init, Collection mapping, in-flight jobs, container stacking,
quarantine) were open DESIGN QUESTIONS DQ-2..DQ-8, catalogued in 17_SAVE_SYSTEM.
[RESOLVED UO-0003] Per the approved development SAVE-DATA RESET policy, prototype
profiles are RESET rather than converted, so DQ-2..DQ-8 are REMOVED from the
current implementation critical path - no prototype-to-V2 conversion logic is
required. DQ-9 (provenance: OwnerUserId / OriginUserId / OriginAnalyzeJobId)
remains OPEN and approval-gated (default NO). The clean V2 schema still begins
its own forward-migration history; expand-migrate-contract applies to all FUTURE
V2 schema changes. See 17_SAVE_SYSTEM and 23_PROJECT_CHANGELOG.

CLEAN V2 START (RESOLVED UO-0003, 2026-07-16)
---------------------------------------------
New profiles start from the clean V2 default schema. Existing prototype profiles
are NOT converted into V2 equivalents. Unknown old records must not be
interpreted or converted into invented V2 state. Progression is stored as stable
upgrade levels/IDs (above); derived capacities/stats are not authoritative saved
values.
