--========================================================
-- UNKNOWN ORE
-- DISPLAY SHELF DESIGN
-- LATEST MERGED PRE-CODE V1 BASELINE
--========================================================

GOAL
----
Shelf is a physical collection display and the primary specialized build system.

SHELF STRUCTURE
---------------
Start: 1 Shelf
Maximum: 4 Shelves
Slots per Shelf: 5
Maximum active display slots: 20

Shelf 1 = Start
Shelf 2 = 50,000 Coins
Shelf 3 = 450,000 Coins
Shelf 4 = 2,000,000 Coins

Shelves are universal.
Shelves are not assigned to Caves.
Duplicate Actual Items may be displayed.

AUTHORITATIVE ITEM METADATA
---------------------------
Shelf assignments operate on unique Actual Item instances (ItemUID), each referencing a
stable ItemId from 25_ITEM_CATALOG_DESIGN. Shelf classification metadata resolves from
ItemConfig after the later Shelf migration. DisplayNames and visual/model/icon references
remain deferred to UO-3600. Shelf does not independently define item identity. Multiple ItemUIDs may
reference the same catalog ItemId without creating a second item definition.

CAVE BUFF IDENTITY
------------------
Crystal = Luck
Lava = Sell Price
Frozen = Analyze Speed
Meteor = Mining Speed

SEPARATE BUFF SCALE
-------------------
Rarity      Luck    Sell       Analyze    Mining
Common      0.5     0.25%      0.5%       0.25%
Uncommon    1.0     0.5%       1.0%       0.5%
Rare        1.5     0.75%      1.5%       0.75%
Epic        2.5     1.25%      2.5%       1.25%
Legendary   4.0     2.0%       4.0%       2.0%
Mythic      6.0     3.0%       6.0%       3.0%

POSITION BUFF MODIFIER
----------------------
A = x1.0
B = x1.1
C = x1.2
D = x1.4

THEORETICAL SPECIALIZED MAXIMUM
-------------------------------
Luck = 168
Sell Price = +84%
Analyze Speed = +168%
Mining Speed = +84%

DERIVED STATE
-------------
Active Buff totals are derived from current Shelf ItemUID mapping.

Do not persist:
CurrentLuck
CurrentSellBonus
CurrentAnalyzeSpeed
CurrentMiningSpeed

NO PRESETS
----------
V1 has no:
Save Build
Load Build
Preset Slot
Quick Build
Auto Equip Best
Build Cooldown
Build Change Cost

This is intentional.

PHYSICAL GAMEPLAY
-----------------
Example:
Use Meteor items for Mining Speed.
Mine.
Return Home.
Interact with Shelf.
Reorganize to Lava items.
Sell with Sell Price Buff.

SHELF EDITOR ENTRY
------------------
Physical Shelf → Manage Shelf.

One editor session may manage all unlocked Shelves.

PRIMARY INPUT
-------------
Select Item
→ Select Slot

Drag and Drop is not required as the primary interaction.

If destination is occupied:
Swap automatically in Draft.

CLIENT-SIDE DRAFT
-----------------
Editing mutates a client Draft only.

Draft Buff Summary updates in real time.

APPLY
-----
Client sends final desired:
ShelfSlot → ItemUID mapping.

Server validates the complete desired state.

Apply is atomic:
All changes succeed
or
No Shelf state changes.

No partial Apply.

FULL INVENTORY SWAP
-------------------
Validate final Inventory state, not intermediate movement.

Inventory 100/100 may still swap:
1 Inventory Item ↔ 1 Shelf Item

if final Inventory count remains valid.

CLEAR
-----
Clear Shelf is available per Shelf.

No Clear All Shelves in V1.

VISITOR INSPECT
---------------
Visitors may inspect public displayed items.

Public inspect may show:
Display Name
Rarity
Cave
Buff value
Owner where appropriate

Do not expose:
ItemUID
OriginAnalyzeJobId
Internal transaction identifiers
