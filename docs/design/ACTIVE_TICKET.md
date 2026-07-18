# UO-3500 — Authoritative Item Catalog Design Refactor

## Goal

Refactor the Unknown Ore design documents so all collectible item identity is owned by one authoritative Item Catalog design.

This ticket is design-only.

Do not modify runtime code, configs, services, UI, models, save data, or gameplay behavior.

Do not invent the missing 87 item names.

## Existing Systems

The current project has:

* `ContainerConfig` controlling Analyze reward pools
* `GemConfig` containing metadata for 9 prototype items
* `CollectionCatalog` manually assigning those items to Collection slots
* Design documents specifying:

  * 4 caves
  * 6 rarities per cave
  * 4 Collection positions per rarity
  * 96 total collectible items
* no completed authoritative 96-item catalog
* no runtime `ItemConfig`

The recent item-source audit found that item identity is currently duplicated across:

* `ContainerConfig`
* `GemConfig`
* `CollectionCatalog`

The intended production source of truth is a central Item Catalog / ItemConfig architecture.

## Requirements

### 1. Create the authoritative design document

Create a new design document:

```text
ReplicatedStorage/Design/18_ITEM_CATALOG_DESIGN
```

If `18_CONTENT_DATABASE` already exists, keep it and use the next available appropriate number instead.

Do not overwrite an unrelated existing design file.

The new document must define:

* the purpose of the authoritative Item Catalog
* the distinction between:

  * Item ID
  * Display Name
  * Cave
  * Container Rarity
  * Item Rarity
  * Collection Rarity
  * Collection Position
  * Reward Pool membership
* the finalized catalog shape:

  * 4 caves
  * 6 rarities
  * 4 positions: A, B, C, D
  * 24 items per cave
  * 96 unique items globally
* one stable Item ID per collectible item
* one Collection position per Item ID
* no duplicate Item IDs
* no gameplay system independently defining item identity

### 2. Define the future catalog schema

Document the intended design fields for each item:

```text
ItemId
DisplayName
Cave
Rarity
CollectionPosition
Description
VisualKey
ModelKey
IconKey
SellTier
Tags
```

Clearly mark which fields are required for the first production catalog and which may be added later.

Do not add random gameplay stats or quality rolls to item identity.

### 3. Preserve unresolved item names

Create the full 96-slot catalog structure, but do not invent final names.

Use explicit unresolved placeholders such as:

```text
CRYSTAL_COMMON_A — NAME PENDING
CRYSTAL_COMMON_B — NAME PENDING
```

The stable placeholder IDs may follow:

```text
<CAVE>_<RARITY>_<POSITION>
```

but the document must clearly state whether these are:

* approved final Item IDs, or
* temporary design identifiers pending approval

Do not silently treat them as final unless existing design already confirms that convention.

### 4. Document the 9 legacy prototype items

Add a migration/reference section for:

* Coal
* Quartz
* Ruby
* Emerald
* Sapphire
* Diamond
* AuroraCrystal
* CelestialQuartz
* SecretCrystalItem

For each, state:

```text
Current Identifier
Current GemConfig Rarity
Current Collection Placement
Current RewardPool References
Final Mapping Status: Unresolved
```

Do not decide their final replacement or catalog position unless current authoritative design already defines it.

### 5. Refactor related design documents

Inspect and update all relevant design files, including at minimum:

* `03_MINING_DESIGN`
* `04_ANALYZE_DESIGN`
* `05_COLLECTION_DESIGN`
* `06_DISPLAY_SHELF_DESIGN`
* `07_ECONOMY_DESIGN`
* `10_DATA_STRUCTURE`
* `11_TECHNICAL_ARCHITECTURE`
* `15_SERVICE_RESPONSIBILITIES`
* `18_CONTENT_DATABASE`
* `99_ROADMAP`

Also update any other design file that independently defines or contradicts collectible item identity.

### 6. Mining and Container rules

Update Mining design so it clearly states:

* Mining creates Unknown Containers.
* Container rarity is not automatically the authoritative source of item identity.
* Reward pools reference Item IDs from the authoritative Item Catalog.
* Reward pool membership does not redefine an item's Cave, rarity, or Collection position.
* Final same-tier versus cross-tier behavior must follow the approved production design.

Identify current cross-tier prototype behavior as prototype-only where applicable.

### 7. Analyze rules

Update Analyze design so it clearly states:

* Analyze outputs one authoritative Item ID.
* Item metadata is resolved from the future runtime `ItemConfig`.
* Analyze must not define display names, Cave ownership, Collection position, or sell metadata itself.
* Results must not fall back to arbitrary unknown ItemType strings in the production architecture.
* Random Quality and random per-instance gameplay stats are not part of finalized item identity unless explicitly approved elsewhere.

Do not change runtime implementation in this ticket.

### 8. Collection rules

Update Collection design so it clearly states:

* Collection is derived from the authoritative Item Catalog.
* Collection must not maintain a second independent list of items.
* Collection groups items by:

  * Cave
  * Rarity
  * Position A/B/C/D
* discovery save data remains keyed by stable Item ID
* catalog completion uses the authoritative catalog count, not hardcoded UI totals

### 9. Shelf rules

Update Shelf design so it clearly states:

* Shelf accepts stable Item IDs from the authoritative catalog.
* Shelf visuals and display metadata resolve from ItemConfig.
* Shelf does not define item names, rarity, Cave, models, or icons independently.

### 10. Economy rules

Update Economy design so it clearly states:

* Sell metadata resolves from ItemConfig or an explicitly referenced economy table.
* Economy does not redefine Item ID, Cave, rarity, or Collection position.
* If `SellTier` is used, document whether the final value is:

  * directly stored in ItemConfig, or
  * derived through an economy balance table.

Do not choose between those approaches unless the existing design already does.

### 11. Architecture rules

Update technical architecture so the intended dependency direction is:

```text
Design Item Catalog
→ Runtime ItemConfig
→ ContainerConfig reward references
→ Analyze
→ Inventory
→ Collection
→ Shelf
→ Economy
→ UI
```

Gameplay systems may consume item data but must not become item-definition authorities.

### 12. Roadmap update

Update the roadmap to add a new content-database phase before further Collection UI completion.

Add future tickets conceptually equivalent to:

```text
UO-3500 — Authoritative Item Catalog Design Refactor
UO-3501 — Runtime ItemConfig Foundation
UO-3502 — Legacy Item Mapping Decision
UO-3503 — Container and Analyze Migration
UO-3504 — Collection Migration
```

Do not create implementation tickets or code in this task.

## Constraints

* Design files only.
* Do not modify Lua runtime code.
* Do not modify Config modules.
* Do not modify UI.
* Do not modify Workspace content.
* Do not migrate save data.
* Do not delete `GemConfig`, `ContainerConfig`, or `CollectionCatalog`.
* Do not invent final item names.
* Do not make unresolved game-design decisions.
* Mark unresolved decisions clearly as `DESIGN QUESTION`.
* Preserve prototype history where useful, but clearly distinguish prototype behavior from production design.
* Avoid duplicating the complete 96-slot catalog across multiple documents.
* The new Item Catalog design must be the only document containing the complete item-position table.

## Deliverables

1. Updated `ACTIVE_TICKET`
2. New authoritative Item Catalog design document
3. Updated related design documents
4. Updated roadmap
5. A concise implementation report containing:

   * files created
   * files modified
   * contradictions resolved
   * prototype rules marked
   * unresolved design questions
   * confirmation that no runtime code was changed

## Acceptance Criteria

* One design document is explicitly authoritative for collectible item identity.
* The complete 96-slot structure exists in that document.
* Missing item names remain clearly unresolved.
* The 9 prototype items are documented without receiving invented final mappings.
* Mining, Analyze, Collection, Shelf, Economy, data, and architecture documents reference the same authority.
* Container rarity, item rarity, Collection rarity, and Collection position are clearly separated.
* No related design document independently maintains a second complete item catalog.
* The roadmap places ItemConfig work before continued Collection completion.
* No runtime code, config, UI, model, or save schema is modified.
* The final report clearly identifies every remaining `DESIGN QUESTION`.

## Implementation Report Checklist

Return:

```text
ACTIVE_TICKET updated: Yes/No

Files created:
- ...

Files modified:
- ...

Authoritative catalog document:
- ...

Contradictions resolved:
- ...

Prototype behavior marked:
- ...

DESIGN QUESTIONS:
- ...

Runtime code changed:
- Must be No

Config modules changed:
- Must be No

Ready for independent review:
- Yes/No
```
