--========================================================
-- UNKNOWN ORE
-- PROGRESSION DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Main progression should move quickly enough to expose all four Cave identities while long-term motivation remains in Shelf and Collection optimization.

MAIN PROGRESSION DEFINITION
---------------------------
Main progression ends at purchasing the Meteor Pickaxe.

It does not mean:
96/96 Collection complete
Perfect Shelf complete
All Mythics acquired

DISCOVERY GATE
--------------
For each Cave, personally mine:
Common / Fragment
Uncommon / Chunk
Rare / Cluster
Epic / Core

After all four are discovered:
The associated Pickaxe becomes purchasable.

Buy Pickaxe with Coins.
The next Cave unlocks.

Heart/Legendary and Relic/Mythic are not required.

TARGET CUMULATIVE ACTIVE PLAYTIME
---------------------------------
Crystal Pickaxe / Lava Cave:
15–30 minutes

Magma Pickaxe / Frozen Cave:
1–1.5 hours

Frost Pickaxe / Meteor Cave:
3–4 hours

Meteor Pickaxe:
6–8 hours

LONG-TAIL TARGETS
-----------------
Strong Shelf Build:
15–30 hours

Strong Specialized Build:
30–60 hours

Near-perfect Build:
100+ hours

PROGRESSION PHILOSOPHY
----------------------
Cave Progression = Fast–Medium
Pickaxe Progression = Medium
Shelf Optimization = Long-term
Collection / Perfect Build = Very Long-term

DO NOT
------
Gate main progression behind Mythic RNG.
Require Heart or Relic discovery for Cave unlock.
Use Collection completion as Pickaxe gate.
Use paid ownership as Cave gate.

PROGRESSION PERSISTENCE (RESOLVED UO-0003, 2026-07-16)
------------------------------------------------------
LOCKED DECISION: durable progression state is STABLE UPGRADE LEVELS / IDS, not
derived runtime values. The profile saves identifiers/levels such as:
  CurrentPickaxeId (or pickaxe level), BackpackUpgradeLevel,
  AnalyzeSlotUpgradeLevel (coin-purchased), ShelfUnlockLevel,
  RefineryUnlocked, HighestCaveUnlocked.

[CORRECTION, 2026-07-16] The prior version of this section also listed an
"AnalyzeMachine upgrade level" as if it were a category distinct from
AnalyzeSlotUpgradeLevel. That phrase had no basis in the locked design and is
removed: 02_CORE_GAMEPLAY ("Coins fund Pickaxes, Backpack, Analyze Slots,
Shelves, and Refinery") and 19_BALANCING_GUIDE's cost tables (PICKAXES,
BACKPACK, ANALYZE SLOTS, SHELVES) confirm Analyze Slots is the ONLY
coin-purchased Analyze progression category. Analyze Speed is a Shelf-derived
buff (06_DISPLAY_SHELF_DESIGN), not a purchasable upgrade level, and must not
be treated as one.

Runtime values are DERIVED from the saved level/ID via BalanceConfig + stable
resolvers and are NEVER stored as the authoritative state. Derived examples:
  mining duration, backpack capacity, Lab capacity, Analyze duration,
  Analyze slot count, shelf count, shelf slot capacity, Luck, Sell Bonus.

Rules:
  - Saved upgrade identifiers are stable and independent of display text.
  - Derived values are deterministic for the same saved progression + config
    version; the server is authoritative for them.
  - Clients may receive public derived values for display only; clients never
    submit authoritative stat values.
  - Balance edits normally change config only, not every player profile.
  - Caps and prerequisites are enforced server-side.
  - Premium entitlement state stays SEPARATE from coin-purchased progression.
  - A future config change that would materially REDUCE previously purchased
    player value must be an explicit balance/migration decision, never silently
    applied.

Resolves DQ-GLOBAL-1. See 10_DATA_STRUCTURE and 17_SAVE_SYSTEM.
