# Item Catalog Design Summary

This file is a repository-facing summary. The detailed Roblox design document remains authoritative when imported from the live project.

## Catalog matrix

- 4 caves
- 6 rarity tiers per cave
- 4 positions per rarity
- 24 slots per cave
- 96 total slots

## Caves

- Crystal
- Lava
- Frozen
- Meteor

## Rarities

- Common
- Uncommon
- Rare
- Epic
- Legendary
- Mythic

## Positions

- A
- B
- C
- D

## Candidate ItemId pattern

```text
<CAVE>_<RARITY>_<POSITION>
```

Example:

```text
CRYSTAL_COMMON_A
```

This convention is a candidate only until DQ-IC-02 is approved.

## Naming state

All 96 display names remain `NAME PENDING`.

## Legacy prototype identifiers

- Coal
- Quartz
- Ruby
- Emerald
- Sapphire
- Diamond
- AuroraCrystal
- CelestialQuartz
- SecretCrystalItem

No production mapping is approved.

## Required ownership

`ItemConfig` is intended to own:

- ItemId
- DisplayName
- Cave
- Item rarity
- Collection placement
- Approved visual references
- Other shared immutable item metadata

Container reward membership and weights remain separate behavior owned by container/reward configuration.
