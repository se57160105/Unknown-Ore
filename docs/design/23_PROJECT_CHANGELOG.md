--========================================================
-- UNKNOWN ORE
-- PROJECT CHANGELOG
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

2026-07-20  UO-3503 RUNTIME CATALOG AUDIT (COMPLETED)
----------------------------------------------------
Completed the documentation-only Runtime Catalog Audit in 27_RUNTIME_CATALOG_AUDIT,
including the verified runtime dependency graph, migration readiness assessment,
recommended migration order, and risk record. Planning documents were synchronized so
UO-3503 refers only to this completed audit; the future runtime migration ticket number
remains TBD and no new migration ticket was created or defined. No runtime file changed.

2026-07-19  UO-3502 LEGACY ITEM DISPOSITION RECORD
---------------------------------------------------
Created 26_LEGACY_ITEM_DISPOSITION as the single authoritative record of all nine
retired legacy prototype Item identifiers (Coal, Quartz, Ruby, Emerald, Sapphire,
Diamond, AuroraCrystal, CelestialQuartz, SecretCrystalItem). Each is documented as
RETIRED with no production ItemId, no production catalog slot, no alias, no automatic
mapping, and no SaveData migration - historical reference only. Consolidated the
duplicated legacy disposition table previously split across 25_ITEM_CATALOG_DESIGN and
14_FIRST_PROTOTYPE_SPEC into this single document; both now reference it instead of
restating it. No production ItemConfig, GemConfig, ContainerConfig, CollectionCatalog,
Analyze, Inventory, save data, reward pools, or UI were changed.

2026-07-19  UO-3501 RUNTIME ITEMCONFIG FOUNDATION
--------------------------------------------------
Approved Item Catalog decisions recorded before implementation:
- DQ-IC-01 RESOLVED: ItemConfig stores Rarity; centralized Economy config derives base
  sell value. ItemConfig has no SellTier or BaseSellValue.
- DQ-IC-02 RESOLVED: production IDs use <Cave>_<Rarity>_<Position> and are stable.
- DQ-IC-03 DEFERRED: all names remain NAME PENDING; visuals move to UO-3600.
- DQ-IC-04 RESOLVED: nine legacy identifiers are retired with no production mapping.
- DQ-IC-05 RESOLVED: RewardPoolConfig exclusively owns membership and weights; intended
  production pools use four same-Cave/same-Rarity items. [SUPERSEDED BY UO-3505-R1,
  2026-07-22: the "at equal weight" selection recorded here at UO-3501 time was stale and
  has been corrected - current production selection is Cave + Container Tier -> Resolve
  Rarity -> Roll Position A/B/C/D using 40/30/20/10 -> Resolve production ItemId. See
  25_ITEM_CATALOG_DESIGN DQ-IC-05 and 27_RUNTIME_CATALOG_AUDIT for the current
  authoritative statement; this entry is preserved as historical record of the original
  UO-3501 decision.]
UO-3501 creates only the validated read-only ItemConfig foundation. No Analyze,
Collection, Shelf, Economy, UI, reward-pool, or save migration is included.

2026-07-18  UO-3500 AUTHORITATIVE ITEM CATALOG DESIGN REFACTOR
----------------------------------------------------------------
25_ITEM_CATALOG_DESIGN is now the single design authority for collectible identity and
the only complete 96-slot table. All 96 names remain NAME PENDING; the slot identifiers
are temporary pending approval. Nine legacy prototype items are documented without final
mappings. Mining, Analyze, Collection, Shelf, Economy, data, architecture, service,
content, naming, implementation, and roadmap documents now reference the same authority.
Current cross-tier reward pools are marked prototype-only. No runtime, config, UI, model,
Workspace, save schema, or save data was changed.

2026-07-16  UO-0003 BLOCKING DECISIONS RESOLVED (PRODUCT SIGN-OFF)
------------------------------------------------------------------
Two blocking product decisions were approved and merged into the Design Bible.

1. DEVELOPMENT SAVE-DATA POLICY = RESET.
   Current saved data is development/prototype data, not production player data.
   When the V2 schema goes active, prototype profiles may be reset (environment-
   scoped) and new profiles start from the clean V2 default. Prototype items /
   containers / collection / analyze jobs / progression / shelves / buffs /
   economy are NOT converted. The reset must be environment-scoped, use a new
   DataStore name/version, record the old store name, keep the old store as a
   rollback/reference point until V2 is verified, and must NEVER become an
   automatic production-deletion path. Applies only because the game has not
   launched with production player data.
   Effect: legacy-conversion DESIGN QUESTIONS DQ-2..DQ-8 are REMOVED from the
   current implementation critical path. DQ-9 (provenance) remains OPEN.

2. PROGRESSION PERSISTENCE = STABLE UPGRADE LEVELS/IDS (derive runtime).
   Save purchased pickaxe ID/level, backpack/storage/analyze/analyze-slot/shelf
   upgrade levels, and other stable IDs. Derive mining duration, capacities,
   analyze duration, slot count, shelf count/slots, Luck, Sell Bonus, etc. from
   BalanceConfig resolvers (server-authoritative; clients get public derived
   values only). Premium entitlements stay separate from coin progression. A
   config change that materially reduces purchased value is an explicit balance/
   migration decision, not silently applied. Resolves DQ-GLOBAL-1.

STILL REQUIRED (unchanged): config foundation (GameEnums / BalanceConfig /
ItemConfig), public projection / DTO, persistence safety (session lease +
observability), clean V2 schema + validator. Expand-migrate-contract still
governs all FUTURE V2 schema changes (not the one-time prototype reset).

Docs updated by UO-0003: 00, 08, 10, 12, 13, 17, 22, 23, 99.
No gameplay / config / network / UI / player-data code was changed.

LATEST MERGED PRE-CODE V1
-------------------------
- Max server players locked at 10.
- 10 private 72x72 plots around Central Hub.
- Homebase target approximately 320x320 studs.
- Plot→Cave Access target <=10–15 seconds.
- Shared Cave environment with personal mining nodes.
- Approximately 6 active personal nodes/player.
- 18–24 candidate spawn points/Cave.
- Main progression target corrected to 6–8 active hours to Meteor Pickaxe.
- Cave progression gated by personal mining discovery of Common/Uncommon/Rare/Epic.
- Pickaxe purchase unlocks next Cave.
- Lab Storage added at 100 base capacity.
- Analyze times changed to tier-based 1m/5m/15m/1h/4h/12h.
- Analyze starts at 2 Slots; Coin progression to 6; premium maximum 8.
- Analyze Speed snapshot at Submit.
- Hidden ResultItemId locked at Submit.
- Manual Reveal required.
- Inventory Full blocks Reveal safely.
- Finish Now added using remaining-time Robux buckets.
- Fusion locked at 5 same Cave/Tier containers.
- Fusion success rates 60/50/40/30.
- Heart→Relic Fusion disabled.
- Failure returns 1 original-tier container.
- Refinery added for Core/Heart/Relic.
- Refinery unlocks on first Frozen Cave entry.
- Refinery position distributions Normal/I/II/III locked.
- Meteor Relic Refine III corrected and locked at 10M Coins.
- Shelf structure locked at 1–4 Shelves, 5 Slots each.
- Shelf Presets removed.
- Separate Buff Scale locked.
- Crystal/Lava/Frozen/Meteor Buff identities locked.
- Physical Draft Shelf UX and atomic Apply locked.
- Actual Item Inventory fixed at 100.
- Unique ItemUID model locked.
- Held Item runtime visual system added.
- Collection registers from Personal Analyze Reveal.
- Collection rewards require Manual Claim.
- Sell System and Quick Sell Common/Uncommon locked.
- Atomic Transaction Flow locked.
- Launch monetization: Analyzer Slot I 99 R$, Slot II 149 R$, Finish Now.
- Expanded Lab +100 remains Playtest Candidate.
- Anomaly Ore moved to Post-Launch Deferred Design.
- Trading/Exchange remains Future.

REMOVED / SUPERSEDED
--------------------
6-player server assumption.
10–16h Meteor Pickaxe target.
10–12M Meteor Relic III target range.
Worker.
Museum Reputation.
Collection EXP.
Restore.
Brush/Research.
Analyze Auto Queue.
Shelf Presets.
Auto Equip Best.
Rebirth.
Mine Storage (UO-1006 interim per-node reward buffer + Collect interaction;
superseded by UO-1006B - Mining rewards enter Mining Backpack directly again).

ARCHITECTURE AUDIT V2 INTEGRATED (PRE-CODE)
-------------------------------------------
Approved Architecture Audit V2 guidance merged into the Design Bible. NO
gameplay decisions changed. Additions are architectural / migration /
networking / persistence / implementation only:
- 16: client replication = public projection/DTO; per-remote validation +
  abuse matrix; request/ack clarification; receipt = platform callback.
- 17: expand-migrate-contract; session lease; DataStore observability;
  deployment/rollback; legacy migration decisions DQ-2..DQ-9.
- 15: TransactionService applied selectively (not a global wrapper);
  Monetization durable receipt ledger.
- 20: rules 11-13 (replay-safety by invariant/conservation; replication =
  projection; test gates).
- 04: result persisted + redacted; derived readiness; non-authoritative
  timers; no re-roll; Finish Now revalidation.
- 10: item authorization = profile membership + Location; provenance
  approval-gated.
- 13: migration sequencing (interface-first; projection-before-persistence;
  config adapter; Crystal-subset-first).
- 00: integration index; expanded pending decisions.
The separate Audit V2 documents are now historical; implement from the Bible.

CORRECTED (VS EARLIER AUDIT V1 FRAMING)
---------------------------------------
- A global TransactionService wrapper on every mutation is NOT required;
  current single-step handlers are naturally idempotent. Use invariants +
  targeted serialization instead.
- The full 96-item catalog is NOT a prerequisite for migration; a final-ID
  Crystal subset suffices to prove mechanics.
- RemoteEvent-only contracts are acceptable (request/ack); not a defect.
- Empty folders / remote renaming are hygiene, handled post-stabilization.
