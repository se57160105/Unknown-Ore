--========================================================
-- UNKNOWN ORE
-- CORE GAMEPLAY DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Define the moment-to-moment loop and intentional player decisions.

PRIMARY LOOP
------------
1. Enter an unlocked Cave.
2. Move between personal mining nodes.
3. Mining starts near the node.
4. Server tracks mining state and completion.
5. On completion, server reads current Luck and rolls the Unknown Container Tier.
6. Container enters Mining Backpack if capacity allows.
7. Return to Homebase and choose an action - these are separate, optional
   actions, not a required sequence, and Deposit is not a prerequisite for
   any of the others:
   - Submit a container to Analyze (see 04_ANALYZE_DESIGN; the final Analyze
     input source - Backpack, Lab Storage, or both - is not yet locked).
   - Deposit containers into Lab Storage for later use.
   - Fuse eligible unrefined containers.
   - Refine eligible Core/Heart/Relic containers.
8. Analyze progresses by FinishAt timestamp, including offline time.
9. Completed jobs become Ready to Reveal.
10. Player manually reveals the result.
11. Actual Item enters Item Inventory if capacity allows.
12. Player may:
   - Hold the 3D model.
   - Keep it for Collection history.
   - Place it on Shelf.
   - Lock it.
   - Sell it.
13. Coins fund Pickaxes, Backpack, Analyze Slots, Shelves, and Refinery.
14. Pickaxe progression unlocks deeper Caves.

INTENTIONAL BUILD-SWITCH GAMEPLAY
---------------------------------
Shelf Buffs are derived from the physical current Shelf mapping.

Example:
Meteor Shelf build → faster mining.
Return Home.
Physically reorganize Shelf into Lava items.
Sell with a stronger Sell Price Buff.

There are no Presets in V1.
Build switching is an intentional physical management action.

PLAYER FAILURE / PRESSURE
-------------------------
Mining Backpack full:
- New mining reward cannot be granted.
- Mining must not silently delete the result.
- Player is directed to Home Base, where Analyze, Deposit into Lab Storage,
  and other approved inventory actions are separate, optional ways to free
  space - none is a required prerequisite for another.
- Mining resumes once Backpack space becomes available.

Lab Storage full:
- Deposit is blocked safely.
- Player must Analyze, Fuse, or otherwise process stored containers.

Analyze Slots full:
- Player may still deposit into Lab Storage while Lab capacity remains.

Actual Item Inventory full:
- Manual Reveal is blocked.
- Analyze Job remains in its slot.
- Hidden ResultItemId remains safe.
- No overflow storage.

SOCIAL LOOP
-----------
Players share the world and Cave environment but do not compete for nodes.
Visitors can inspect another player's displayed collection.
Visitors cannot interact with or remove the owner's items.

V1 DOES NOT INCLUDE
-------------------
Trading
Item dropping
PvP
Shared node stealing
Auto Claim
Auto Queue
Auto Sell
Shelf Presets
