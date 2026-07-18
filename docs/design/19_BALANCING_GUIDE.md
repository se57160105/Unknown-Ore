--========================================================
-- UNKNOWN ORE
-- MASTER BALANCE AND BALANCING GUIDE
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

BALANCECONFIG RULE
------------------
BalanceConfig.lua is the runtime single source of truth for numerical balance.

Do not copy balance numbers into Services.

SERVER
------
Max Players = 10

DROP RATES
----------
Common 60%
Uncommon 25%
Rare 10%
Epic 4%
Legendary 0.9%
Mythic 0.1%

PICKAXES
--------
Starter 10s / Free
Crystal 9s / 12,500
Magma 8s / 90,000
Frost 7s / 500,000
Meteor 6s / 2,500,000
Minimum Mining Time = 2s

BACKPACK
--------
30 Start
40 = 3,000
50 = 12,000
60 = 50,000
75 = 200,000
100 = 750,000

LAB
---
Base Capacity = 100

ANALYZE TIME
------------
Common 1m
Uncommon 5m
Rare 15m
Epic 1h
Legendary 4h
Mythic 12h

ANALYZE SLOTS
-------------
2 Start
3 = 20,000
4 = 125,000
5 = 600,000
6 = 2,000,000
Premium I +1 / 99 R$
Premium II +1 / 149 R$
Maximum 8

SHELVES
-------
1 Start
Shelf 2 = 50,000
Shelf 3 = 450,000
Shelf 4 = 2,000,000
5 slots each
20 maximum

SEPARATE BUFF SCALE
-------------------
Common:
Luck 0.5 / Sell 0.25% / Analyze 0.5% / Mining 0.25%

Uncommon:
1.0 / 0.5% / 1.0% / 0.5%

Rare:
1.5 / 0.75% / 1.5% / 0.75%

Epic:
2.5 / 1.25% / 2.5% / 1.25%

Legendary:
4.0 / 2% / 4% / 2%

Mythic:
6.0 / 3% / 6% / 3%

POSITION BUFF MODIFIER
----------------------
A x1.0
B x1.1
C x1.2
D x1.4

THEORETICAL SPECIALIZED MAX
---------------------------
Luck 168
Sell +84%
Analyze +168%
Mining +84%

FUSION
------
Input 5
Failure recovery 1 original-tier container

Fragment→Chunk 60%
Chunk→Cluster 50%
Cluster→Core 40%
Core→Heart 30%
Heart→Relic disabled

REFINERY DISTRIBUTION
---------------------
Normal 40/30/20/10
I 35/30/22/13
II 30/30/24/16
III 25/30/25/20

Meteor Relic:
I 1.5M
II 4M
III 10M

SELL BASE
---------
Common 25
Uncommon 75
Rare 250
Epic 1,000
Legendary 5,000
Mythic 25,000

POSITION SELL
-------------
A x1.0
B x1.2
C x1.5
D x2.0

CAVE SELL
---------
Crystal x1
Lava x2
Frozen x5
Meteor x12

COLLECTION CRYSTAL BASE
-----------------------
Common 1,000
Uncommon 3,000
Rare 10,000
Epic 40,000
Legendary 150,000
Mythic 500,000

COLLECTION CAVE MULTIPLIER
--------------------------
Crystal x1
Lava x2
Frozen x4
Meteor x8

FINISH NOW
----------
<=5m 5 R$
<=15m 9 R$
<=30m 15 R$
<=1h 29 R$
<=2h 49 R$
<=4h 79 R$
<=8h 119 R$
<=12h 149 R$

PROGRESSION TARGET
------------------
Crystal/Lava 15–30m
Magma/Frozen 1–1.5h
Frost/Meteor 3–4h
Meteor Pickaxe 6–8h
Strong Shelf 15–30h
Strong Specialized 30–60h
Near-perfect 100h+

PLAYTEST PRIORITIES
-------------------
Validate 6–8h Meteor target.
Validate Coin source/sink curve.
Validate Refinery affordability.
Validate Mythic frequency.
Validate Shelf power gap.
Validate Analyze pressure.
Validate Lab 100 pressure.
Validate Finish Now conversion.
Evaluate Expanded Lab +100 candidate.
