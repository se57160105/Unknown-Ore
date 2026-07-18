--========================================================
-- UNKNOWN ORE
-- SAVE SYSTEM
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Persist durable progression safely while keeping runtime state derived or ephemeral.

SAVE
----
SchemaVersion
Meta
Economy
Progression
CaveDiscovery
Storage
Analyze Jobs
Actual Items
Shelf mapping
Collection
Entitlements
Settings
Statistics

DO NOT SAVE
-----------
Personal node runtime state
HeldItemUID
Spawned held models
Current derived Buff totals
Remaining Analyze seconds

ANALYZE TIME
------------
Save:
StartedAt
FinishAt
ResultItemId

Do not save RemainingSeconds.

On load:
Compare current time with FinishAt.

SHELF BUFFS
-----------
Save Shelf ItemUID mapping.

Recalculate Buffs from:
Shelf mapping
ItemConfig
BalanceConfig

SESSION LIFECYCLE
-----------------
Load profile.
Validate/migrate.
Bind to player session.
Autosave.
Release on leave/session release.
Handle PlayerRemoving.
Handle BindToClose.

DO NOT SAVE EVERY TRANSACTION
-----------------------------
Gameplay mutation updates in-memory session data.
Persistence follows the profile/session save lifecycle.

MIGRATIONS
----------
Use SchemaVersion.
Migrations must be deterministic.
Do not silently delete unknown durable items during migration.

PROGRESSION STORAGE DECISION - LOCKED (RESOLVED UO-0003, 2026-07-16)
-------------------------------------------------------------------
[Was "PENDING PROGRESSION STORAGE DECISION".]
LOCKED: Save stable upgrade levels/IDs. Derive capacities/counts/stats from
BalanceConfig via resolvers (never save derived values as authoritative).
Premium entitlements stay separate from coin progression. Resolves DQ-GLOBAL-1.
See 08_PROGRESSION_DESIGN and 10_DATA_STRUCTURE.

DEVELOPMENT SAVE-DATA RESET POLICY (RESOLVED UO-0003, 2026-07-16)
----------------------------------------------------------------
Current saved player data belongs to a DEVELOPMENT PROTOTYPE, not production
player data. When the V2 schema becomes active:
  * Existing development/test profiles MAY be reset.
  * New profiles start from the clean V2 default schema.
  * Legacy prototype items/containers/collection/analyze jobs/progression/
    shelves/buffs/economy do NOT require conversion into V2 equivalents.
  * Unknown old records must NOT be interpreted or converted into invented V2
    state.
Safety (binding on the FUTURE implementation ticket; nothing is reset now):
  * The reset is ENVIRONMENT-SCOPED and deliberate; identify the exact dev
    DataStore/environment being reset; never reset unrelated DataStores.
  * Use a NEW DataStore name/version (or another explicit dev reset mechanism);
    record the old DataStore name before switching; keep the old store as a
    rollback/reference point until the V2 schema is verified.
  * Never perform broad destructive deletion from normal player-load code.
  * This reset mechanism must NEVER become an automatic production-data deletion
    path. The policy applies ONLY because Unknown Ore has not launched with
    production player data.
The clean V2 schema still begins a NEW forward-migration history: normal forward
migration support is required from its first production-capable SchemaVersion,
and expand-migrate-contract (below) governs all FUTURE V2 schema changes.


--========================================================
-- ARCHITECTURE ADDENDUM (AUDIT V2 - APPROVED, PRE-CODE)
--========================================================
Authoritative. Persistence, migration, and rollout are ONE deliverable.
Gameplay decisions are unchanged.

SESSION OWNERSHIP (LEASE) - required before any schema migration
---------------------------------------------------------------
Raw GetAsync/SetAsync is not safe cross-server. Either adopt a maintained
profile/session library (and document its failure behaviour) OR implement a
lease with ALL of:
  * lock token + owning server/job identity written into the record;
  * lease expiry + heartbeat/renewal;
  * ownership-checked UpdateAsync on EVERY save (never blind SetAsync);
  * stale-lock recovery (steal only after expiry + grace) and force-load
    policy;
  * load cancellation when ownership cannot be acquired;
  * teleport / reserved-server handoff;
  * defined behaviour when release fails at shutdown.
Verify with a two-server contention test and a stale-lock recovery test
BEFORE migrating schema.

DATASTORE FAILURE MODES + OBSERVABILITY
---------------------------------------
Data safety is operational, not just schema. Required:
  * retries with exponential backoff + jitter; request-budget-aware autosave;
  * dirty-save coalescing;
  * payload-size + serialization validation before write (full-profile
    save/replication magnifies one bad field);
  * corrupt-profile quarantine (never hard-delete);
  * telemetry: load/save/migration latency + error counts, last-success
    metadata, shutdown final-save failure reporting, and before/after
    migration conservation counts.

EXPAND - MIGRATE - CONTRACT (no big-bang schema cutover)
-------------------------------------------------------
A migrated profile must never be handed to old service code, and old servers
may overlap during deploy (reserved/private servers lag). Therefore:
  1. EXPAND: add SchemaVersion, new groups, the public projection (see 16),
     and compatibility accessors WITHOUT deleting old fields.
  2. MIGRATE: move one vertical slice at a time to the new accessors, with
     dual-read (and narrowly justified dual-write). Preserve unknown fields.
  3. Reader/writer compatibility rule: reject or safely hand off profiles
     NEWER than the running server supports; older servers must preserve
     fields they do not understand.
  4. CONTRACT: remove legacy fields only AFTER no supported live server reads
     them and the rollback window has closed.
Deployment gates: expand-compatible release first; canary cohort; migration
dry-run with before/after conservation counts; write gate + kill switch;
retained rollback-compatible window; contract after fleet convergence.

LEGACY-DATA CONVERSION DECISIONS  [RESOLVED/SUPERSEDED UO-0003, 2026-07-16]
--------------------------------------------------------------------------
STATUS (UO-0003): The approved DEVELOPMENT SAVE-DATA RESET POLICY (above)
supersedes prototype-to-V2 legacy conversion. Because prototype profiles are
RESET (not converted):
  * DQ-GLOBAL-1 is RESOLVED - persist upgrade levels/IDs; derive the rest.
  * DQ-2..DQ-8 are REMOVED from the current implementation critical path - no
    production-grade prototype conversion logic is required. They are retained
    below only as HISTORICAL context and as a template for any FUTURE post-
    launch legacy migration (which would then need the fixtures/quarantine/
    conservation counters described).
  * DQ-9 (provenance) remains OPEN and approval-gated (default NO).
The original catalog is preserved below for reference (historical; not blocking):

"Preserve durable items" is a safety goal, not a mapping. Each item below is a
DESIGN QUESTION requiring product sign-off; each answered rule needs fixtures,
before/after examples, an unmapped-data quarantine path, and conservation
counters proving no durable record vanished. [Historical - see STATUS above.]
  DQ-GLOBAL-1  Progression persistence (save upgrade level/ID vs derive) -
               see PENDING PROGRESSION STORAGE DECISION above.
  DQ-2  Legacy actual-item identity: old gem-name items with random
        Quality/Value -> 96-ID cave/rarity/POSITION catalog (position not
        present in old data). Grandfather vs map vs compensate. Position
        cannot be invented deterministically.
  DQ-3  Grade removal without value shock: removing Quality/Value changes
        sell payouts; decide revaluation/compensation so players see no
        silent loss.
  DQ-4  CaveDiscovery initialization: none exists and it must NOT be inferred
        from owned items (see 10_DATA_STRUCTURE). Decide initial state for
        existing players (policy, not inference).
  DQ-5  Collection mapping: old Collection[gemType] -> per-item discovery +
        rarity/cave/global claim eligibility; avoid duplicate reward grants.
  DQ-6  In-flight/Ready analyze jobs lacking ResultItemId: resolve into a
        locked result, refund, or complete once under old rules - never a
        hidden re-roll.
  DQ-7  Container conversion: old unique list entries -> normal containers
        stacked by Cave:Tier (refined stay unique); recompute capacity.
  DQ-8  Unmapped data / unknown fields: quarantine path; never silently
        delete.
  DQ-9  Provenance: whether OwnerUserId/OriginUserId/OriginAnalyzeJobId are
        added in V1 (default NO - see 10_DATA_STRUCTURE); if approved, define
        writer, immutability, validation, and UnknownLegacy origin.
