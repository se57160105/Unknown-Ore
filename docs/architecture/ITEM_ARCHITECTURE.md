# Item Architecture

## Target dependency direction

```text
Item Catalog Design
        ↓
ItemConfig
        ↓
ContainerConfig / AnalyzeService
        ↓
InventoryService / DataService
        ↓
CollectionService
        ↓
ShelfService
        ↓
EconomyService
        ↓
UI
```

## Responsibilities

### ItemConfig

Owns immutable, shared collectible metadata.

### ContainerConfig

Owns unknown-container behavior, reward membership, and reward weights. It references valid ItemIds.

### AnalyzeService

Consumes a container result and produces a valid ItemId plus approved instance attributes.

### InventoryService

Stores item instances. Each analyzed item references one valid ItemId.

### CollectionService

Stores discovery state keyed by ItemId and uses ItemConfig for ordering and presentation metadata.

### ShelfService

Stores placed item instances or references and derives set eligibility from ItemConfig.

### EconomyService

Calculates value from approved item/economy metadata. It must not define item identity.

### UI

Renders data returned from authoritative systems. It must not maintain duplicate catalog tables.

## Validation requirements

The future ItemConfig foundation should detect:

- duplicate ItemIds
- missing required fields
- invalid cave
- invalid rarity
- duplicate cave/rarity/position combinations
- missing catalog combinations
- invalid visual references, when those fields become required
