--========================================================
-- UNKNOWN ORE
-- NETWORK ARCHITECTURE
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Networking must treat the client as untrusted.

CLIENT REQUESTS INTENT
----------------------
Examples:
RequestDeposit
RequestFusion
RequestRefine
RequestAnalyzeSubmit
RequestReveal
RequestShelfApply
RequestSellSelected
RequestQuickSell
RequestCollectionClaim
RequestHoldItem
RequestPutAway
RequestProgressionPurchase

SERVER DECIDES
--------------
Ownership
Capacity
Eligibility
Costs
Drop results
Analyze results
Sell payout
Shelf validity
Collection reward
Entitlement grant

REQUEST ID
----------
Mutating requests should include RequestId where appropriate.

Transaction result cache target:
60 seconds.

Repeated RequestId:
Return cached result.
Do not mutate twice.

MINING
------
Mining completion is server-driven.
Do not grant mining drops from a client "complete mining" reward request.

SHELF APPLY
-----------
Client sends final desired ShelfSlot → ItemUID mapping.

Server validates:
Item exists
Ownership
Duplicate UID references
Unlocked Shelf/Slot
Item state
Final Inventory capacity

Then applies all-or-nothing.

QUICK SELL
----------
Client sends Quick Sell intent/rule.

Server scans eligible Actual Items.

Do not trust client bulk UID selection for Quick Sell.

ANALYZE
-------
Never replicate hidden ResultItemId before Reveal.

MONETIZATION
------------
Client purchase prompt is not proof of grant.
Receipt processing is server-side and idempotent.

VISITOR SECURITY
----------------
Inspect is read-only.
Never allow visitor UI to call owner mutation operations using owner IDs.
Server always resolves acting player from the Remote sender.


--========================================================
-- ARCHITECTURE ADDENDUM (AUDIT V2 - APPROVED, PRE-CODE)
--========================================================
Authoritative. Merges approved architectural guidance into this doc.
Gameplay decisions are unchanged. Cross-references avoid duplication.

CLIENT REPLICATION = PUBLIC PROJECTION ONLY (supersedes full-profile snapshot)
-----------------------------------------------------------------------------
The server MUST replicate to a client only an explicit PUBLIC PROJECTION (DTO)
of that player's own profile - never the raw profile table.
The projection ALLOWLISTS public fields and REDACTS every server-only field:
the hidden analyze ResultItemId (see 04_ANALYZE_DESIGN), receipt/entitlement
internals, internal instance IDs, migration metadata, and any future
server-only field.
Reason: the reveal secret is safe only while it is absent from the replicated
payload. Once a hidden result is PERSISTED in the profile (04 + 17), a
full-profile replicate would leak it. "Server-only" is enforced by the
projection, not by which table the field lives in.
A serialization test MUST assert that no redacted field ever appears in a
client payload. Full-profile replication is NOT permitted.

REMOTE VALIDATION + ABUSE CONTROL (required for EVERY remote)
------------------------------------------------------------
Server authority is not complete merely because the server computes rewards.
Every client value is hostile until validated. For each remote (existing and
planned) specify the following, and validate BEFORE any lock/allocation/costly
work:
  * schema/type - typeof on every field; reject NaN/infinity on numbers;
    enum allowlists for IDs; reject malformed instances.
  * bounds - table entry-count and depth caps BEFORE iteration/allocation;
    payload-size ceiling; quantity limits.
  * authorization - acting player = Remote sender only (never a client ID).
  * state preconditions - re-resolve against server-owned state (ownership,
    capacity, slot readiness, affordability).
  * spatial - proximity check where the action is location-bound.
  * rate limit / cooldown - per player, per action.
  * idempotency - defined per remote (see REPLAY SAFETY in 20_CODING_RULES
    and the service duties in 15_SERVICE_RESPONSIBILITIES).
  * response + audit log.
RequestId is attacker-controlled: bound its length/format and scope it per
player; a duplicate or oversized RequestId must never allocate unboundedly.

REQUEST/ACK OVER REMOTEEVENTS IS SUFFICIENT
-------------------------------------------
The locked requirement is an authoritative success/error result WITH request
correlation - not a specific Remote class. A correlated RemoteEvent request/ack
carrying a RequestId satisfies it. RemoteFunctions are NOT required and are not
a validation mechanism; if ever introduced they need identical validation.
Renaming remotes to intent style is cleanup, not a migration blocker.

RECEIPT PROCESSING IS A PLATFORM CALLBACK, NOT A NORMAL REMOTE
-------------------------------------------------------------
ProcessReceipt is a MarketplaceService callback, retried later and possibly on
a different server. Its idempotency MUST use a DURABLE ledger keyed by the
platform PurchaseId (see 15_SERVICE_RESPONSIBILITIES MonetizationService and
17_SAVE_SYSTEM), never the in-memory request cache.
