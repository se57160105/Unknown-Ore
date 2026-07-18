--========================================================
-- UNKNOWN ORE
-- COLLECTION DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Collection is the primary long-term completion system.

DISCOVERY RULE
--------------
Collection registers only from Personal Analyze Reveal.

Do not reconstruct Collection from current Inventory ownership.

Future received/traded items must not automatically count unless a future design explicitly changes this rule.

AUTHORITATIVE CATALOG
---------------------
Collection is a projection of the authoritative catalog in 25_ITEM_CATALOG_DESIGN and
must not maintain a second independent item list. It groups catalog entries by Cave,
intrinsic Rarity, and CollectionPosition A/B/C/D. Discovery save data is keyed by stable
ItemId; Inventory ownership is not a discovery source.

Cave and global completion denominators are derived from the authoritative runtime
ItemConfig. The approved catalog currently yields 24 per Cave and 96 globally, but UI and
claim validation must not maintain separate hardcoded content totals. Repeat discovery of
the same ItemId does not create another catalog entry.

DATA
----
Recommended:

Collection = {
    DiscoveredItems = {},
    ClaimedRaritySets = {},
    ClaimedCaveRewards = {},
    ClaimedGlobalReward = false,
}

RARITY SET
----------
Each Cave/Rarity has four item positions:
A
B
C
D

4/4 creates a claimable Rarity Set reward.

Rewards are Manual Claim.
No Auto Claim.

CRYSTAL BASE RARITY-SET REWARDS
-------------------------------
Common      1,000 Coins
Uncommon    3,000 Coins
Rare        10,000 Coins
Epic        40,000 Coins
Legendary   150,000 Coins
Mythic      500,000 Coins

CAVE REWARD MULTIPLIERS
-----------------------
Crystal x1
Lava x2
Frozen x4
Meteor x8

CAVE COMPLETION
---------------
24/24 items in one Cave.

Reward type:
Physical Trophy.

GLOBAL COMPLETION
-----------------
96/96 items.

Reward concept:
Unknown Artifact
+
Plot Effect

COLLECTION DOES NOT
-------------------
Give gameplay Buffs.
Gate Pickaxe progression.
Use Collection EXP.
Use Museum Reputation.

COLLECTION UI
-------------
Show:
- Cave tabs.
- 24-item grid.
- Unknown silhouette for undiscovered items.
- Rarity grouping.
- A/B/C/D set completion.
- Claimable reward state.
- Cave completion progress.
- Global 96/96 progress.

Claim must be a server-authoritative transaction.
