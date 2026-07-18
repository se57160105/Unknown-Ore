--========================================================
-- UNKNOWN ORE
-- UI / UX DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
UI must be mobile-friendly, readable, and make storage/reveal state obvious.

MINING HUD
----------
Show:
Coins
Mining Backpack used/capacity
Current Cave
Current Pickaxe
Mining progress when active
Backpack Full warning

Do not show hidden drop outcome before completion.

LAB STORAGE UI
--------------
Show:
Used / Capacity
Container groups by Cave and Tier
Unrefined vs Refined state
Actions:
Analyze
Fusion
Refinery where eligible

Lab Storage base: 100.

ANALYZE UI
----------
Show all unlocked slots.
Slot states:
EMPTY
ANALYZING
READY TO REVEAL

Analyzing:
Container identity
Start/Finish context
Remaining display timer
Finish Now action and current product price

Ready:
Clear Reveal action

Never expose ResultItemId before Reveal.

REVEAL UX
---------
Manual Reveal.
Reveal should be the visual reward moment.

If Inventory is full:
Show clear blocking message.
Do not remove the Job.

INVENTORY UI
------------
Actual Item Inventory:
100 fixed capacity.

Group by ItemId.
Filters:
Cave
Rarity
Locked where useful

Actions:
Hold / Put Away
Lock / Unlock
Inspect
Shelf management entry where appropriate
Sell Selected through Sell flow

HELD ITEM UX
------------
Held item remains in Inventory.
Show Hold / Put Away state.
Quick Sell must skip Held Item.

SHELF UX
--------
Entry:
Interact with physical Shelf → Manage Shelf.

Editor:
All unlocked Shelves
Inventory list
Select Item → Select Slot
Automatic Draft swap
Real-time Buff Preview
Apply
Cancel
Clear current Shelf

No Presets.
No Auto Equip Best.
No Clear All.

COLLECTION UI
-------------
Cave tabs.
24-item grid.
Unknown silhouettes.
Rarity sections.
A/B/C/D set completion.
Manual Claim.
24/24 Cave progress.
96/96 Global progress.

SELL UI
-------
Group by ItemId.
Cave filter.
Quantity:
-10 / -1 / +1 / +10
Sell Selected
Quick Sell Common
Quick Sell Uncommon

No Sell All.

SOCIAL INSPECT
--------------
Visitors can inspect Shelf/Trophy displays.
Read-only.
Never expose internal IDs.

MOBILE RULES
------------
Large touch targets.
Avoid drag-only core interactions.
Use select-then-select flows.
Critical destructive actions require clear confirmation.
Server response must drive final success/error state.
