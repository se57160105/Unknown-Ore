--========================================================
-- UNKNOWN ORE
-- FIRST PROTOTYPE SPEC
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

PURPOSE
-------
The first playable prototype proves the current core architecture, not the full content catalog.

PROTOTYPE MUST PROVE
--------------------
This numbered proof checklist describes Prototype Runtime coverage, not production
dependency order. Production item work follows:
Item Catalog Design -> ItemConfig -> Legacy Migration -> Analyze Migration ->
Collection Migration -> Shelf -> Economy -> UI.

1. Personal mining node ownership.
2. Server-authoritative mining completion.
3. Mining Speed snapshot.
4. Luck read at completion.
5. Unknown Container grant.
6. Mining Backpack capacity.
7. Deposit into Lab Storage.
8. Analyze Submit.
9. Analyze Speed snapshot.
10. Hidden ResultItemId.
11. FinishAt timestamp progress.
12. Manual Reveal.
13. Unique Actual Item Instance.
14. Actual Item Inventory capacity.
15. Hold/Put Away 3D model.
16. Shelf Draft editing.
17. Atomic Shelf Apply.
18. Derived Buff calculation.
19. Sell with transaction-time Sell Buff.
20. Collection registration from Personal Reveal.

MINIMUM CONTENT
---------------

PROTOTYPE RUNTIME - NOT PRODUCTION DESIGN:
The current implementation is Crystal-only, uses the nine legacy identifiers, partial
CollectionCatalog placement, prototype reward pools, generic/placeholder visuals, random
Quality and derived per-instance Value, and does not consume production ItemConfig.
UO-3501 adds an isolated ItemConfig foundation without changing those prototype consumers.

Production ItemIds use the locked <Cave>_<Rarity>_<Position> format. The nine legacy
prototype identifiers are retired with no production mapping or catalog slot - see
26_LEGACY_ITEM_DISPOSITION for the full identifier list and per-identifier disposition.
Runtime removal is deferred to a later explicit migration ticket.

DO NOT HARD-CODE PROTOTYPE ASSUMPTIONS
--------------------------------------
Do not design services around:
One Cave forever
One Shelf forever
One Analyze Slot forever
No Refinery forever

The service interfaces must support V1 expansion.

NOT REQUIRED FOR FIRST PROTOTYPE
--------------------------------
Full 96 items
All four Cave art sets
Final UI art
Final tutorial
Monetization purchase testing
Anomaly Ore
Trading
