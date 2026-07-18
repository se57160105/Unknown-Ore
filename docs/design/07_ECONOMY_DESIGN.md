--========================================================
-- UNKNOWN ORE
-- ECONOMY AND SELL DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Coins must retain value through permanent progression and recurring Refinery spending.

CURRENCY
--------
Coins only.

No premium in-game currency in V1.

COIN SOURCES
------------
Sell Actual Items
Collection Claims

PERMANENT COIN SINKS
--------------------
Pickaxe purchases
Mining Backpack upgrades
Analyze Slot unlocks
Shelf unlocks
Other locked BalanceConfig gameplay upgrades

Currently modeled permanent sink total:
9,362,500 Coins

RECURRING COIN SINK
-------------------
Refinery

BASE SELL VALUES
----------------
Common      25
Uncommon    75
Rare        250
Epic        1,000
Legendary   5,000
Mythic      25,000

ITEM CATALOG AUTHORITY
----------------------
Sell metadata resolves from future runtime ItemConfig or from an explicitly referenced
economy balance table. Economy consumes stable ItemIds from 25_ITEM_CATALOG_DESIGN and
does not redefine ItemId, Cave, intrinsic Rarity, or CollectionPosition.

DESIGN QUESTION DQ-IC-01:
Decide whether SellTier is stored directly in ItemConfig or derived through an explicitly
referenced economy balance table. Until approved, SellTier is an unresolved catalog-to-
economy reference; this document's rarity/position/cave values remain balance inputs and
do not become independent identity definitions.

POSITION SELL MODIFIER
----------------------
A = x1.0
B = x1.2
C = x1.5
D = x2.0

CAVE SELL MULTIPLIER
--------------------
Crystal = x1
Lava = x2
Frozen = x5
Meteor = x12

SELL FORMULA
------------
floor(
    BaseValue
    * CaveMultiplier
    * PositionSellModifier
    * (1 + SellBonus / 100)
)

SellBonus is read from active Shelf state when the server processes the Sell transaction.

SELL ELIGIBILITY
----------------
Only Actual Items with:
Location == INVENTORY
and
IsLocked == false

may be sold.

Cannot sell:
Unknown Container
Refined Container
Analyze Job
Shelf Item
Locked Actual Item

SELL UI
-------
Group Actual Items by ItemId.

Support:
Cave filter
Quantity selection
-10
-1
+1
+10
Sell Selected
Quick Sell Common
Quick Sell Uncommon

No Sell All Inventory in V1.

QUICK SELL
----------
Server scans eligible items itself.

Do not trust a client-supplied bulk UID list.

Exclude:
Locked Items
Shelf Items
Held Item

INTENTIONAL SELL BUILD
----------------------
Sell Price Buff is read at transaction time.

Therefore:
Mine with Mining Build
→ Return Home
→ Change Shelf to Sell Build
→ Sell

is intended gameplay.

REFINERY ECONOMY
----------------
Refinery is the primary repeatable Coin sink.

Meteor Relic prices:
Refine I = 1,500,000
Refine II = 4,000,000
Refine III = 10,000,000

All other Refinery prices must come from BalanceConfig.

ECONOMY PLAYTEST
----------------
Measure:
Coins/hour by Cave and Pickaxe.
Coins/hour by Sell Build strength.
Permanent upgrade affordability.
Refinery usage rate.
Time to 2.5M Meteor Pickaxe.
Post-Meteor Coin inflation.
Collection reward contribution.
