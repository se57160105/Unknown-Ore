--========================================================
-- UNKNOWN ORE
-- CODING RULES
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

1. SERVER AUTHORITATIVE
-----------------------
Never trust client balances, ownership, drop results, result items, sell totals, or reward eligibility.

2. CONFIG-DRIVEN
----------------
Numerical balance belongs in BalanceConfig.
Collectible identity is designed in 25_ITEM_CATALOG_DESIGN and implemented in ItemConfig;
no service or UI maintains a second item catalog.
Stable shared enum domains belong in GameEnums. Production collectible ItemIds use the
locked <Cave>_<Rarity>_<Position> format and belong to the authoritative catalog/ItemConfig,
not an independent enum or gameplay system.

3. SERVICE RESPONSIBILITY
-------------------------
One service, one clear domain responsibility.
TransactionService does not absorb business logic.

4. ATOMIC MUTATION
------------------
VALIDATE
→ CALCULATE / BUILD PLAN
→ APPLY
→ VERIFY CRITICAL INVARIANTS
→ RETURN

5. UNIQUE IDS
-------------
Actual Items use ItemUID.
Refined Containers use ContainerUID.
Analyze Jobs use JobId.
Mutating client requests use RequestId where appropriate.

6. NO HIDDEN CLIENT RESULT
--------------------------
Do not replicate Analyze ResultItemId before Reveal.

7. NO SCATTERED HARD-CODED BALANCE
----------------------------------
Do not put times, prices, Buff scales, drop rates, or capacity tables directly in Services.

8. DERIVE STATE
---------------
Do not persist Shelf Buff totals or Remaining Analyze seconds.

9. FAIL SAFELY
--------------
Inventory Full must not delete Reveal result.
Storage Full must not silently discard items.
Duplicate RequestId must not double-apply.

10. V1 SCOPE
------------
Do not implement:
Anomaly Ore
Trading
Shelf Presets
Auto Equip Best
Rebirth

unless design baseline is explicitly changed.


--========================================================
-- ARCHITECTURE ADDENDUM (AUDIT V2 - APPROVED, PRE-CODE)
--========================================================

11. REPLAY SAFETY BY INVARIANT
------------------------------
Every mutation: VALIDATE -> RESERVE/REMOVE -> GRANT -> COMMIT.
Items, Containers, and Coins are conserved across every successful mutation
(no creation or loss beyond the intended delta).
A rejected or duplicate request causes NO state change.
Single-step handlers are naturally idempotent when they remove/mark the target
BEFORE crediting and re-check server-owned state - preserve that property.
Add per-player serialization ONLY for multi-step/yielding operations (deposit,
fusion, refine, cross-storage moves). A global mutation wrapper is NOT required
(see 15_SERVICE_RESPONSIBILITIES TransactionService).

12. REPLICATION = PUBLIC PROJECTION (extends rule 6)
---------------------------------------------------
Replicate only an allowlisted PUBLIC PROJECTION of the player's own profile;
redact every server-only field (hidden ResultItemId, receipt/entitlement
internals, internal IDs, migration metadata). Persisting a server-only field
must never expose it. Full-profile replication is not permitted.
See 16_NETWORK_ARCHITECTURE.

13. TEST GATES BEFORE ADVANCING A MIGRATION SLICE
-------------------------------------------------
Required proof to advance: conservation checks; migration fixtures for every
historical DataVersion + malformed/unknown fields; old-reader/new-writer
compatibility; two-server contention + stale-lock recovery; leave/rejoin/
shutdown during analyze and during save; duplicate/replay per mutation;
cross-server receipt replay; capacity-full + partial-failure for deposit,
fusion, refine, reveal, shelf apply, and sell.
See 17_SAVE_SYSTEM and 13_IMPLEMENTATION_PLAN.
