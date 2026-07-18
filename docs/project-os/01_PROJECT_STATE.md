# 01 — Project State

Last updated: 2026-07-18  
Project: Unknown Ore  
Platform: Roblox  
Development model: Solo developer with AI-assisted design, implementation, and review

## Current phase

**Content Database and Item Architecture Refactor**

Collection implementation is paused until the authoritative item catalog and runtime `ItemConfig` foundation are established.

## Current active epic

**UO-3500 series — Authoritative Item Catalog and Runtime Migration**

Planned order:

1. UO-3500 — Authoritative Item Catalog Design Refactor
2. UO-3501 — Runtime ItemConfig Foundation
3. UO-3502 — Legacy Item Mapping Decision
4. UO-3503 — Container and Analyze Migration
5. UO-3504 — Collection Migration
6. Resume Collection UI completion after the migration is stable

## Recently completed

### UO-3500 — Authoritative Item Catalog Design Refactor

Status: Completed; awaiting or undergoing independent review.

Reported result:

- Created authoritative design document `25_ITEM_CATALOG_DESIGN`.
- Defined 96 unique catalog slots:
  - Crystal: 24
  - Lava: 24
  - Frozen: 24
  - Meteor: 24
- Each cave contains:
  - 6 rarity tiers
  - 4 positions per rarity: A, B, C, D
- All item names remain `NAME PENDING`.
- Candidate slot identifiers remain temporary pending approval.
- Nine legacy prototype identifiers are documented without invented final mappings.
- Runtime code changed: No.
- Config modules changed: No.

## Locked game baseline

### Caves

1. Crystal Cave
2. Lava Cave
3. Frozen Cave
4. Meteor Cave

### Catalog size

- 24 analyzed items per cave
- 96 analyzed items globally
- 6 rarity tiers per cave
- 4 item positions per rarity

### Rarity tiers

1. Common
2. Uncommon
3. Rare
4. Epic
5. Legendary
6. Mythic

### Unknown container mapping

- Fragment → Common
- Chunk → Uncommon
- Cluster → Rare
- Core → Epic
- Heart → Legendary
- Relic → Mythic

No production cross-tier overlap is approved. Existing cross-tier runtime reward pools are prototype-only.

### Core gameplay loop

1. Player stands near a mining node.
2. Mining occurs automatically.
3. Unknown containers enter mine storage.
4. Mining stops when mine storage is full.
5. Player manually collects containers into the player bag.
6. Player analyzes a container.
7. Analyze reveals the actual collectible item.
8. Player sells, collects, or displays analyzed items.
9. Display shelf sets grant approved buffs.

### Timing baseline

- Mining time: 10 seconds
- Analyze time baseline: 10 seconds per item
- Analyze speed can be improved through upgrades

### Inventory and storage

- Mine storage and player bag are separate.
- Mine storage has capacity and stops mining when full.
- Player must manually collect from mine storage.
- Player cannot drop items while in the field.
- Player bag has a fixed capacity; final exact value remains unresolved between 50 and 100.
- No worker system.
- No auto-claim.
- No offline analyze queue.
- No Analyze Slot system.

### Currency and economy

- Single currency: Coins
- Approved sinks include:
  - Cave unlocks
  - Analyze-time upgrades
  - Luck upgrades
  - Sell-price upgrades
  - Shelf unlocks
  - Shelf-slot unlocks
  - Analyze or restore costs, if retained by final design

### Approved buff categories

- Move speed
- Analyze-time reduction
- Luck increase
- Sell-price increase

Bag capacity is a base system decision, not a shelf buff.

### Explicitly excluded buff or upgrade categories

- Critical mine
- Mining power
- Auto claim
- Shelf effect as a generic buff type
- Worker automation

### Display shelf

- Located at the player homebase.
- Universal; shelves are not physically tied to caves.
- Player starts with one shelf.
- Additional shelves may be unlocked through Coins or Robux; final structure remains unresolved.
- Each shelf has limited slots; exact slot count remains unresolved.
- Duplicate items may be displayed.
- Set rules may use cave, rarity, or other approved catalog metadata.
- Shelf systems consume item metadata but do not own item identity.

## Current architecture direction

The intended dependency flow is:

```text
Game Design
    ↓
ItemConfig
    ↓
ContainerConfig / Analyze
    ↓
Inventory
    ↓
Collection
    ↓
Shelf
    ↓
Economy
```

`ItemConfig` will become the authoritative runtime source for collectible identity and shared item metadata.

Gameplay systems must not maintain independent item catalogs.

## Current prototype reality

Current runtime identity is fragmented across:

- `GemConfig`
- `ContainerConfig`
- `CollectionCatalog`

Known legacy runtime identifiers:

1. Coal
2. Quartz
3. Ruby
4. Emerald
5. Sapphire
6. Diamond
7. AuroraCrystal
8. CelestialQuartz
9. SecretCrystalItem

Current prototype limitations:

- Crystal-only content
- Nine collectible identifiers
- Partial Collection catalog
- Prototype cross-tier reward pools
- Generic or incomplete visuals
- Random prototype quality/value behavior
- No authoritative runtime `ItemConfig`

## Open design questions

- DQ-IC-01 — Store `SellTier` directly in `ItemConfig` or derive it from an economy table.
- DQ-IC-02 — Approve the candidate stable ItemId convention.
- DQ-IC-03 — Approve final item names and visual/model/icon references.
- DQ-IC-04 — Map, replace, or retire each legacy identifier.
- DQ-IC-05 — Approve production reward-pool membership and weights.
- DQ-BAG-01 — Final player bag capacity: 50 or 100.
- DQ-SHELF-01 — Slots per shelf.
- DQ-SHELF-02 — Final shelf acquisition structure.
- DQ-BAL-01 — Final rarity rates and luck formula/cap.
- DQ-BAL-02 — Final buff values and caps.
- DQ-CAVE-01 — Final numeric cave-identity bonuses.

## Immediate next action

Complete an independent UO-3500 documentation review. Do not begin runtime migration until the review result and blocking design decisions are known.
