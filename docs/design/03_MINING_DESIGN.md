--========================================================
-- UNKNOWN ORE
-- MINING DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Mining must be simple, readable, server-authoritative, and compatible with specialized Shelf builds.

PERSONAL MINING NODES
---------------------
Nodes are personal per player.
The Cave environment is shared.

Target:
- Approximately 6 active personal nodes per player.
- Approximately 18–24 candidate spawn points per Cave.
- Personal node respawn approximately 1–2 seconds.
- Average movement between nodes approximately 2–4 seconds.

Players do not consume another player's node.

CAVE ORDER
----------
CRYSTAL
→ LAVA
→ FROZEN
→ METEOR

UNKNOWN CONTAINER MAPPING
-------------------------
Fragment = Common
Chunk = Uncommon
Cluster = Rare
Core = Epic
Heart = Legendary
Relic = Mythic

Normal containers never cross tiers.

Example:
METEOR:CORE can reveal only Meteor Epic A/B/C/D.

AUTHORITATIVE ITEM CATALOG
--------------------------
Mining creates Unknown Containers; it does not create or define Actual Item identity.
Container rarity is a property of the container and is not the authoritative source of
an item's Cave, intrinsic rarity, or Collection position.

Reward pools reference stable ItemIds defined by 25_ITEM_CATALOG_DESIGN and, at runtime,
ItemConfig. Pool membership is only a reward reference and never redefines catalog
metadata. The approved production rule for normal containers is same-tier membership as
stated above.

PROTOTYPE RUNTIME - NOT PRODUCTION DESIGN:
Current ContainerConfig cross-tier memberships are prototype-only history, not approved
production content.

DESIGN DECISION DQ-IC-05 - RESOLVED [UPDATED UO-3505-R1, stale equal-weight statement
corrected]:
Production RewardPool composition and weights belong exclusively to RewardPoolConfig.
Production selection is:
  Cave + Container Tier
  -> Resolve Rarity
  -> Roll Position A/B/C/D using 40/30/20/10
  -> Resolve production ItemId
Selection is NOT equal-weight/uniform across the four same-Cave/same-Rarity ItemIds. See
25_ITEM_CATALOG_DESIGN DQ-IC-05 and 18_CONTENT_DATABASE NORMAL POSITION DISTRIBUTION for
the authoritative statement. UO-3501 does not create or migrate RewardPoolConfig;
RewardPoolConfig itself was implemented by UO-3505/UO-3505-R1 and changes no current
gameplay pool.

BASE DROP RATES
---------------
Common      60.0%
Uncommon    25.0%
Rare        10.0%
Epic         4.0%
Legendary    0.9%
Mythic       0.1%

Luck calculation must use BalanceConfig.
Do not duplicate the formula inside MiningService.

PICKAXES
--------
Starter Pickaxe
Base Mining Time: 10s
Cost: Free

Crystal Pickaxe
Base Mining Time: 9s
Cost: 12,500 Coins

Magma Pickaxe
Base Mining Time: 8s
Cost: 90,000 Coins

Frost Pickaxe
Base Mining Time: 7s
Cost: 500,000 Coins

Meteor Pickaxe
Base Mining Time: 6s
Cost: 2,500,000 Coins

MINING TIME
-----------
FinalMiningTime =
max(
    MinimumMiningTime,
    PickaxeBaseTime / (1 + MiningSpeedBonus / 100)
)

MinimumMiningTime = 2 seconds.

SNAPSHOT RULES
--------------
Mining Speed:
Snapshot when mining starts.

Luck:
Read at mining completion immediately before the Drop Tier roll.

This is intentional.
A Shelf change after mining starts does not change that mining timer.
A Shelf change before completion may affect the Luck roll.

SERVER AUTHORITY
----------------
Server owns:
- Active personal node state.
- Mining start eligibility.
- Mining FinishAt/state.
- Completion.
- Luck read.
- Tier roll.
- Container grant.
- Node respawn.

Client may show animation/UI but does not grant rewards.

REWARD FLOW (LOCKED, UO-1006B)
------------------------------
On Mining completion, the generated Unknown Container enters the Mining
Backpack directly and immediately. There is no intermediate per-node holding
area: Mining Backpack is the only destination for a freshly-generated reward.

Mining continues while the Mining Backpack has space. See BACKPACK FULL below
for what happens once it does not.

MINE STORAGE - SUPERSEDED (UO-1006B)
-------------------------------------
UO-1006 implemented an interim per-personal-node "Mine Storage" buffer (reward
held at the node; a separate Collect interaction moved it into the Backpack).
That system is REMOVED and is not part of the approved design:
- Mining rewards do not buffer at the node.
- There is no node storage capacity.
- There is no Collect interaction at a Mining node (no "press E to collect").
- "Mine Storage" is not a synonym for Mining Backpack or Lab Storage.
UO-1006 is superseded by UO-1006B. See 23_PROJECT_CHANGELOG.

BACKPACK FULL
-------------
Do not grant a reward that cannot fit.
Do not silently delete the reward.
Mining flow must clearly indicate Backpack Full and direct the player to Homebase.
Mining must not start or continue while the Mining Backpack is full; it may
resume once Backpack space is available (see 02_CORE_GAMEPLAY PLAYER FAILURE /
PRESSURE, and 04_ANALYZE_DESIGN LAB STORAGE for the Home Base actions that
free Backpack space: Analyze, or Deposit into Lab Storage).

CAVE DISCOVERY
--------------
CaveDiscovery registers when the player's own mining roll produces a tier for that Cave.

Required progression tiers:
Fragment/Common
Chunk/Uncommon
Cluster/Rare
Core/Epic

Does not register from:
Analyze
Collection
Future Trading
Future Anomaly Ore
Transferred Actual Items

Heart and Relic are not required for main Cave progression.
