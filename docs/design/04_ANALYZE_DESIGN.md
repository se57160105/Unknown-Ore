--========================================================
-- UNKNOWN ORE
-- ANALYZE DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Analyze is the main reveal pipeline and must preserve anticipation without creating storage deadlocks.

LAB STORAGE
-----------
Base capacity: 100 containers.

Lab Storage is separate from:
- Mining Backpack
- Actual Item Inventory

Capacity counts:
Unrefined Container stacks
+
Refined Container Instances

Flow:
Mining Backpack
→ Deposit
→ Lab Storage
→ Fusion / Refinery / Analyze

The Flow above shows Deposit into Lab Storage as ONE supported path toward
Fusion/Refinery/Analyze - it is not, by itself, a decision that Analyze may
ONLY consume from Lab Storage. Deposit is not a prerequisite for Analyze
unless a future Analyze design explicitly requires it; whether Analyze
accepts Mining Backpack items directly, Lab Storage items, or both, is not
yet locked and is left to a future Analyze ticket to decide explicitly.

Lab Storage solves:
Mining Backpack Full + Analyze Slots Full.

ANALYZE SLOTS
-------------
Start: 2 Slots

Coin progression:
3 Slots = 20,000 Coins
4 Slots = 125,000 Coins
5 Slots = 600,000 Coins
6 Slots = 2,000,000 Coins

Premium:
Extra Analyzer Slot I = +1
Extra Analyzer Slot II = +1

Maximum total = 8 Slots.

ANALYZE TIMES
-------------
Fragment / Common      1 minute
Chunk / Uncommon       5 minutes
Cluster / Rare         15 minutes
Core / Epic            1 hour
Heart / Legendary      4 hours
Relic / Mythic         12 hours

BalanceConfig is the runtime source of truth.

ANALYZE COST
------------
Analyze is free.

Refinery is the optional recurring Coin sink before Analyze.

NO AUTO QUEUE
-------------
V1 has no automatic Analyze Queue.
The player manually chooses and submits each container.

SUBMIT FLOW
-----------
VALIDATE
→ READ ACTIVE ANALYZE SPEED
→ CALCULATE DURATION
→ ROLL HIDDEN RESULT POSITION
→ RESOLVE ResultItemId
→ CREATE ANALYZE JOB
→ CONSUME SOURCE CONTAINER

RESULT TIMING
-------------
ResultItemId is rolled and locked at Submit.

The client must not receive ResultItemId before Reveal.

AUTHORITATIVE RESULT IDENTITY
-----------------------------
Analyze resolves exactly one authoritative ItemId from reward-pool references to the
catalog in 25_ITEM_CATALOG_DESIGN. ItemConfig resolves ItemId, DisplayName, Cave,
Rarity, and CollectionPosition. Names and visuals remain deferred to UO-3600; sell value
belongs to centralized Economy configuration. Analyze must not define or override them.

Production results must not fall back to arbitrary unknown ItemType strings. A reward
reference that is absent from ItemConfig is invalid and must fail closed without creating
an unidentified Actual Item. Random Quality and random per-instance gameplay stats are
not part of finalized item identity unless explicitly approved in authoritative design.

PROTOTYPE RUNTIME - NOT PRODUCTION DESIGN:
The current Analyze implementation still consumes legacy GemType identifiers and creates
random Quality plus a derived per-instance Value. These behaviors remain temporarily for
compatibility; they are not production item identity and are not changed by UO-3500.

ANALYZE SPEED SNAPSHOT
----------------------
Analyze Speed is snapshotted at Submit.

Changing the Frozen Shelf build later does not modify an existing job.

JOB DATA
--------
Recommended fields:

JobId
CaveId
TierId
RefinementLevel
StartedAt
FinishAt
ResultItemId

FinishAt is the source of truth.

Do not save RemainingSeconds.

OFFLINE PROGRESS
----------------
Analyze continues through timestamps while the player is offline.

READY TO REVEAL
---------------
When:
os.time() >= FinishAt

The Job becomes Ready to Reveal.

Do not Auto Reveal.

MANUAL REVEAL
-------------
Player must press Reveal.

Reveal creates a unique Actual Item Instance.

Recommended fields:
ItemUID
ItemId
OwnerUserId
OriginUserId
OriginAnalyzeJobId
ObtainedAt
Location
IsLocked

INVENTORY FULL
--------------
If Actual Item Inventory is full:
- Reveal fails safely.
- Job remains in Analyze Slot.
- ResultItemId remains locked.
- Item is not lost.
- No overflow storage.

FINISH NOW
----------
Finish Now is a repeatable Developer Product flow.

One purchase finishes one selected active Analyze Job.

Effect:
FinishAt = now

It does not:
- Auto Reveal
- Improve odds
- Change Refinement distribution
- Finish all jobs

TARGET REMAINING-TIME BUCKETS
-----------------------------
<= 5m   = 5 R$
<= 15m  = 9 R$
<= 30m  = 15 R$
<= 1h   = 29 R$
<= 2h   = 49 R$
<= 4h   = 79 R$
<= 8h   = 119 R$
<= 12h  = 149 R$

Displayed price must come from Roblox Product Metadata.
Do not hard-code the visible Robux price in UI.


--========================================================
-- IMPLEMENTATION NOTES (AUDIT V2 - APPROVED, PRE-CODE)
--========================================================
These refine HOW to implement the decisions above; the decisions are unchanged.

RESULT IS PERSISTED AT SUBMIT, AND SERVER-ONLY
----------------------------------------------
ResultItemId is rolled and locked at Submit (RESULT TIMING) and PERSISTED in
the job (JOB DATA). Because it is now persisted, it MUST be REDACTED from
client replication by the public projection - see 16_NETWORK_ARCHITECTURE and
rule 12 in 20_CODING_RULES.
NEVER RE-ROLL: a Ready job whose result is missing from memory was already
locked at Submit - reload the persisted ResultItemId; do not roll a fresh one.

READINESS IS DERIVED; TIMERS ARE NON-AUTHORITATIVE
--------------------------------------------------
Ready = (os.time() >= FinishAt), evaluated on read/claim. Any timer/callback is
ONLY a non-authoritative notification: capture UserId + JobId, reacquire the
live session, compare job identity, and resolve idempotently. A delayed
callback must never mutate after session release or complete the wrong job.

FINISH NOW APPLY
----------------
Client Finish Now intent is advisory. On apply, revalidate the target job
server-side before setting FinishAt = now. Receipt idempotency uses the durable
ledger (15_SERVICE_RESPONSIBILITIES / 17_SAVE_SYSTEM), not the request cache.

PROVENANCE FIELDS
-----------------
OwnerUserId / OriginUserId (JOB DATA and reveal fields) are future-provenance.
V1 inclusion is APPROVAL-GATED - see 10_DATA_STRUCTURE and DQ-9 in
17_SAVE_SYSTEM. In V1, item authorization is server-side profile membership +
Location, not a stored owner field.
