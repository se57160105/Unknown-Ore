--========================================================
-- UNKNOWN ORE
-- AI DEVELOPMENT RULES
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

FOR CLAUDE / AI CODING ASSISTANTS

SOURCE PRIORITY
---------------
1. ACTIVE_TICKET and the relevant Latest Merged Design Bible files.
   For collectible identity, 25_ITEM_CATALOG_DESIGN is authoritative.
2. BalanceConfig / ItemConfig / GameEnums after created; runtime config implements rather
   than overrides approved design authority.
3. Current implementation.
4. Old code/comments only if they do not conflict.

DO NOT REVIVE REMOVED SYSTEMS
-----------------------------
Worker
Museum Reputation
Collection EXP
Restore
Brush/Research step
Analyze Auto Queue
Shelf Presets
Auto Equip Best
Rebirth

DO NOT ASSUME OLD VALUES
------------------------
Server max is 10, not 6.
Meteor Pickaxe target is 6–8 active hours, not 10–16.
Meteor Relic Refine III is 10,000,000 Coins, not a 10–12M range.

V1 DEFERRED
-----------
Do not implement Anomaly Ore.
Do not implement Trading/Exchange.

ARCHITECTURE RULE
-----------------
Server authoritative.
Config-driven balance.
Unique item instances.
Atomic mutation.
Request idempotency.
Derived Buff state.
Timestamp Analyze state.

WHEN DESIGN IS UNCLEAR
----------------------
Do not invent a new major system.
Mark the conflict and ask for a design decision.

PROGRESSION PERSISTENCE - LOCKED (RESOLVED UO-0003, 2026-07-16)
--------------------------------------------------------------
[Was "PENDING DECISION"; approved via UO-0003, not silently locked.]
Persist stable Upgrade Level/ID; derive capacities/counts/stats from
BalanceConfig. Derived values are never the authoritative saved state; premium
entitlements stay separate from coin progression.
See 08_PROGRESSION_DESIGN, 10_DATA_STRUCTURE, 17_SAVE_SYSTEM.
