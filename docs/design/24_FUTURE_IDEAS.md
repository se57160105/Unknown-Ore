--========================================================
-- UNKNOWN ORE
-- FUTURE IDEAS
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

ANOMALY ORE
-----------
Status:
Post-Launch Candidate / Deferred Design.

Current concept only:
Cave-specific paid random ore.
Possible tiers:
Rare
Epic
Legendary

No Mythic in current concept.

Must Analyze before Reveal.
Does not count toward CaveDiscovery.
Does not unlock Pickaxe progression.

Before implementation:
Economy Simulation
Shelf Power Simulation
Collection acceleration review
Expected Coins per Robux review
Current Roblox paid-random-item compliance review
Schema design
Receipt-flow design

V1 MUST NOT IMPLEMENT
---------------------
Anomaly Schema
Anomaly Receipt Flow
Anomaly UI
Anomaly Analyze SourceType
Anomaly Analytics

Optional future feature flag:
FutureFeatures = {
    AnomalyOre = false,
}

SPECIMEN EXCHANGE / TRADING
---------------------------
Status:
Future.

Current architecture preserves:
ItemUID
OwnerUserId
OriginUserId
OriginAnalyzeJobId

V1 does not include:
Trade UI
Listings
Exchange
Player-to-player transfer

EXPANDED LAB STORAGE
--------------------
Potential:
+100 Lab Capacity
Base 100 → 200
Target price concept: 149 Robux

Status:
Playtest Candidate.

Do not launch until Lab Storage pressure is tested.

TUTORIAL
--------
Final Tutorial design is deferred until physical map and main UI flow are playable.
