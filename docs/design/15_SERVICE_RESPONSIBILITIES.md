--========================================================
-- UNKNOWN ORE
-- SERVICE RESPONSIBILITIES
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

SHARED ITEM CATALOG AUTHORITY
-----------------------------
25_ITEM_CATALOG_DESIGN is the design authority and UO-3501 ItemConfig is the
runtime authority for production collectible identity. Services consume stable ItemIds and catalog
metadata; no service defines a separate name, Cave, Rarity, CollectionPosition, model,
icon, or sell identity.

Analyze resolves a referenced ItemId. ItemService creates instances that reference it.
Collection derives catalog positions and counts from it. Shelf resolves display/buff
metadata from it. Sell resolves catalog/economy metadata without redefining identity.

TransactionService
------------------
Per-player mutation lock.
Request idempotency.
Execute/error/unlock/result cache.
Does not own domain business logic.
APPLICATION (Audit V2): a toolkit applied SELECTIVELY, not a global wrapper
around every mutation. Each domain operation must be replay-safe by invariant
on its own (validate -> reserve/remove -> grant -> commit; conserve
items/containers/coins; no state change on a rejected or duplicate request) -
see 20_CODING_RULES. Add per-player serialization ONLY around operations that
can interleave (multi-step / yielding: deposit, fusion, refine, cross-storage
moves). Use RequestId result-caching ONLY for retryable request/ack flows.
Durable purchase idempotency is SEPARATE (see MonetizationService).

MiningService
-------------
Personal nodes.
Mining start/completion.
Mining Speed snapshot.
Luck read.
Drop roll.
Container grant.
CaveDiscovery event/source.

StorageService
--------------
Backpack/Lab capacity.
Deposit.
Container storage access.
Capacity resolvers.

FusionService
-------------
Validate same Cave/Tier unrefined inputs.
Calculate batch outcomes.
Apply atomic Fusion plan.

RefineryService
---------------
Unlock state.
Eligible tiers.
Refinement cost/distribution.
Create Refined Container Instance.
Prevent repeat refine/fusion.

AnalyzeService
--------------
Slot availability.
Submit.
Analyze Speed snapshot.
Hidden result resolution.
FinishAt.
Ready to Reveal.
Reveal eligibility.

ItemService
-----------
ItemUID.
Actual Item creation.
Inventory state.
Lock.
First Legendary/Mythic Auto Lock.
Item lookup/validation.

HeldItemService
---------------
Runtime HeldItemUID.
Spawn/attach held model.
Put Away.
Auto Put Away integration.

ShelfService
------------
Shelf unlock/slots.
Draft Apply validation.
Final desired mapping.
Atomic mapping mutation.

BuffService
-----------
Derive Luck, Sell, Analyze, Mining Buff totals from Shelf mapping.

SellService
-----------
Sell eligibility.
Quantity/group operations.
Quick Sell scan.
Transaction-time Sell Buff.
Coin payout.

CollectionService
-----------------
Register Personal Analyze Reveal.
Rarity set completion.
Manual claims.
Cave/global completion.

ProgressionService
------------------
Pickaxe purchase.
Discovery gate.
Cave unlock.
Backpack upgrades.
Analyze Slot upgrades.
Shelf unlocks.
Refinery unlock state.

MonetizationService
-------------------
Premium Analyzer Slot entitlements.
Finish Now purchase intent.
Developer Product receipt grant.
Receipt idempotency.
DURABLE RECEIPT LEDGER (Audit V2): receipts are a MarketplaceService callback,
retried later and possibly cross-server; idempotency MUST use a durable ledger
keyed by the platform PurchaseId, atomically coupled to the grant (or a
replayable pending grant). Return granted ONLY after durable success. Client
Finish Now intent is advisory - revalidate the target analyze job on apply.
The in-memory request cache is NOT sufficient here.
See 17_SAVE_SYSTEM and 16_NETWORK_ARCHITECTURE.

PlayerDataService
-----------------
Session/profile lifecycle.
Autosave.
Release.
Data access boundary.
