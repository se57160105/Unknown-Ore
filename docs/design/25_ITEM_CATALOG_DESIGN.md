25_ITEM_CATALOG_DESIGN
Status: AUTHORITATIVE DESIGN
Ticket: UO-3500 - Authoritative Item Catalog Design Refactor

PURPOSE

This document is the single design authority for collectible item identity and for the
complete item-to-Collection-position catalog. Future runtime ItemConfig data implements
this design. Gameplay systems consume catalog data; they do not independently define it.

AUTHORITY AND DEPENDENCY DIRECTION

Design Item Catalog
-> Runtime ItemConfig
-> ContainerConfig reward references
-> Analyze
-> Inventory
-> Collection
-> Shelf
-> Economy
-> UI

No downstream system may redefine an ItemId's display name, Cave, intrinsic rarity,
Collection position, visual references, or economy reference.

TERM BOUNDARIES

- ItemId: Stable, globally unique collectible identity used by saves and system references.
- DisplayName: Player-facing name resolved from the authoritative catalog; not an identity key.
- Cave: The item's intrinsic catalog ownership. Reward sources do not change it.
- ContainerRarity: A property of an Unknown Container and an input to reward selection.
  It is not the item's identity or intrinsic rarity.
- ItemRarity: The item's intrinsic rarity in the catalog.
- CollectionRarity: The grouping rarity shown by Collection. It resolves from the
  authoritative item's Rarity and is not a separate item-definition authority.
- CollectionPosition: Exactly one slot A, B, C, or D within the item's Cave and Rarity.
- RewardPool membership: A reference from a reward source to an ItemId. Membership does
  not redefine the item's Cave, Rarity, CollectionRarity, or CollectionPosition.

FINALIZED CATALOG SHAPE

- Caves: Crystal, Lava, Frozen, Meteor
- Rarities per Cave: Common, Uncommon, Rare, Epic, Legendary, Mythic
- Positions per Cave/Rarity: A, B, C, D
- Items per Cave: 24
- Unique items globally: 96
- Every collectible has exactly one stable ItemId and one Collection position.
- ItemIds are globally unique; Cave/Rarity/Position tuples are globally unique.
- No gameplay system may independently define collectible identity.

FUTURE RUNTIME ITEMCONFIG SCHEMA

Fields required in the first production catalog:
- ItemId
- DisplayName
- Cave
- Rarity
- CollectionPosition

Integration fields that must be resolved before their consuming production system ships:
- VisualKey
- ModelKey
- IconKey
- SellTier

Fields that may be added later without changing identity:
- Description
- Tags

SellTier is an economy reference, not a random per-instance stat.

DESIGN QUESTION DQ-IC-01:
Decide whether SellTier is stored directly in ItemConfig or derived through an explicitly
referenced economy balance table. This document does not choose between those approaches.

Random Quality and random per-instance gameplay stats are not part of finalized item
identity unless separately approved in authoritative design.

ITEM ID STATUS

The identifiers below follow <CAVE>_<RARITY>_<POSITION> only to make all 96 design slots
unambiguous. They are TEMPORARY DESIGN IDENTIFIERS PENDING APPROVAL, not approved final
ItemIds. NAME PENDING is not a DisplayName.

DESIGN QUESTION DQ-IC-02:
Approve final stable ItemIds for all 96 slots, either by approving this identifier pattern
or by supplying replacements before runtime ItemConfig production work.

DESIGN QUESTION DQ-IC-03:
Approve the 96 final DisplayNames and the required visual/model/icon references. No missing
names or asset keys are invented by UO-3500.

COMPLETE 96-SLOT CATALOG

## CRYSTAL
CRYSTAL_COMMON_A - NAME PENDING
CRYSTAL_COMMON_B - NAME PENDING
CRYSTAL_COMMON_C - NAME PENDING
CRYSTAL_COMMON_D - NAME PENDING
CRYSTAL_UNCOMMON_A - NAME PENDING
CRYSTAL_UNCOMMON_B - NAME PENDING
CRYSTAL_UNCOMMON_C - NAME PENDING
CRYSTAL_UNCOMMON_D - NAME PENDING
CRYSTAL_RARE_A - NAME PENDING
CRYSTAL_RARE_B - NAME PENDING
CRYSTAL_RARE_C - NAME PENDING
CRYSTAL_RARE_D - NAME PENDING
CRYSTAL_EPIC_A - NAME PENDING
CRYSTAL_EPIC_B - NAME PENDING
CRYSTAL_EPIC_C - NAME PENDING
CRYSTAL_EPIC_D - NAME PENDING
CRYSTAL_LEGENDARY_A - NAME PENDING
CRYSTAL_LEGENDARY_B - NAME PENDING
CRYSTAL_LEGENDARY_C - NAME PENDING
CRYSTAL_LEGENDARY_D - NAME PENDING
CRYSTAL_MYTHIC_A - NAME PENDING
CRYSTAL_MYTHIC_B - NAME PENDING
CRYSTAL_MYTHIC_C - NAME PENDING
CRYSTAL_MYTHIC_D - NAME PENDING

## LAVA
LAVA_COMMON_A - NAME PENDING
LAVA_COMMON_B - NAME PENDING
LAVA_COMMON_C - NAME PENDING
LAVA_COMMON_D - NAME PENDING
LAVA_UNCOMMON_A - NAME PENDING
LAVA_UNCOMMON_B - NAME PENDING
LAVA_UNCOMMON_C - NAME PENDING
LAVA_UNCOMMON_D - NAME PENDING
LAVA_RARE_A - NAME PENDING
LAVA_RARE_B - NAME PENDING
LAVA_RARE_C - NAME PENDING
LAVA_RARE_D - NAME PENDING
LAVA_EPIC_A - NAME PENDING
LAVA_EPIC_B - NAME PENDING
LAVA_EPIC_C - NAME PENDING
LAVA_EPIC_D - NAME PENDING
LAVA_LEGENDARY_A - NAME PENDING
LAVA_LEGENDARY_B - NAME PENDING
LAVA_LEGENDARY_C - NAME PENDING
LAVA_LEGENDARY_D - NAME PENDING
LAVA_MYTHIC_A - NAME PENDING
LAVA_MYTHIC_B - NAME PENDING
LAVA_MYTHIC_C - NAME PENDING
LAVA_MYTHIC_D - NAME PENDING

## FROZEN
FROZEN_COMMON_A - NAME PENDING
FROZEN_COMMON_B - NAME PENDING
FROZEN_COMMON_C - NAME PENDING
FROZEN_COMMON_D - NAME PENDING
FROZEN_UNCOMMON_A - NAME PENDING
FROZEN_UNCOMMON_B - NAME PENDING
FROZEN_UNCOMMON_C - NAME PENDING
FROZEN_UNCOMMON_D - NAME PENDING
FROZEN_RARE_A - NAME PENDING
FROZEN_RARE_B - NAME PENDING
FROZEN_RARE_C - NAME PENDING
FROZEN_RARE_D - NAME PENDING
FROZEN_EPIC_A - NAME PENDING
FROZEN_EPIC_B - NAME PENDING
FROZEN_EPIC_C - NAME PENDING
FROZEN_EPIC_D - NAME PENDING
FROZEN_LEGENDARY_A - NAME PENDING
FROZEN_LEGENDARY_B - NAME PENDING
FROZEN_LEGENDARY_C - NAME PENDING
FROZEN_LEGENDARY_D - NAME PENDING
FROZEN_MYTHIC_A - NAME PENDING
FROZEN_MYTHIC_B - NAME PENDING
FROZEN_MYTHIC_C - NAME PENDING
FROZEN_MYTHIC_D - NAME PENDING

## METEOR
METEOR_COMMON_A - NAME PENDING
METEOR_COMMON_B - NAME PENDING
METEOR_COMMON_C - NAME PENDING
METEOR_COMMON_D - NAME PENDING
METEOR_UNCOMMON_A - NAME PENDING
METEOR_UNCOMMON_B - NAME PENDING
METEOR_UNCOMMON_C - NAME PENDING
METEOR_UNCOMMON_D - NAME PENDING
METEOR_RARE_A - NAME PENDING
METEOR_RARE_B - NAME PENDING
METEOR_RARE_C - NAME PENDING
METEOR_RARE_D - NAME PENDING
METEOR_EPIC_A - NAME PENDING
METEOR_EPIC_B - NAME PENDING
METEOR_EPIC_C - NAME PENDING
METEOR_EPIC_D - NAME PENDING
METEOR_LEGENDARY_A - NAME PENDING
METEOR_LEGENDARY_B - NAME PENDING
METEOR_LEGENDARY_C - NAME PENDING
METEOR_LEGENDARY_D - NAME PENDING
METEOR_MYTHIC_A - NAME PENDING
METEOR_MYTHIC_B - NAME PENDING
METEOR_MYTHIC_C - NAME PENDING
METEOR_MYTHIC_D - NAME PENDING

CATALOG INVARIANTS

- Exactly 4 Caves x 6 Rarities x 4 Positions = 96 catalog entries.
- No duplicate ItemId.
- No duplicate Cave/Rarity/CollectionPosition tuple.
- Exactly one Collection position per ItemId.
- Collection completion derives its totals from the authoritative runtime catalog.
- Other design documents must reference this table rather than copy it.

LEGACY PROTOTYPE MIGRATION / REFERENCE

These are current prototype facts only. Existing identifiers and placements are not
approved final mappings into the 96-slot production catalog.

Current Identifier: Coal
Current GemConfig Rarity: Common
Current Collection Placement: Crystal / Common / A
Current RewardPool References: Common
Final Mapping Status: Unresolved

Current Identifier: Quartz
Current GemConfig Rarity: Common
Current Collection Placement: Crystal / Common / B
Current RewardPool References: Common, Uncommon
Final Mapping Status: Unresolved

Current Identifier: Ruby
Current GemConfig Rarity: Uncommon
Current Collection Placement: Crystal / Uncommon / A
Current RewardPool References: Common, Uncommon, Rare
Final Mapping Status: Unresolved

Current Identifier: Emerald
Current GemConfig Rarity: Rare
Current Collection Placement: Crystal / Rare / A
Current RewardPool References: Uncommon, Rare, Epic
Final Mapping Status: Unresolved

Current Identifier: Sapphire
Current GemConfig Rarity: Rare
Current Collection Placement: Crystal / Rare / B
Current RewardPool References: Rare, Epic
Final Mapping Status: Unresolved

Current Identifier: Diamond
Current GemConfig Rarity: Epic
Current Collection Placement: Crystal / Epic / A
Current RewardPool References: Epic, Legendary
Final Mapping Status: Unresolved

Current Identifier: AuroraCrystal
Current GemConfig Rarity: Legendary
Current Collection Placement: Crystal / Legendary / A
Current RewardPool References: Legendary, Mythic
Final Mapping Status: Unresolved

Current Identifier: CelestialQuartz
Current GemConfig Rarity: Legendary
Current Collection Placement: Crystal / Legendary / B
Current RewardPool References: Legendary, Mythic
Final Mapping Status: Unresolved

Current Identifier: SecretCrystalItem
Current GemConfig Rarity: Mythic
Current Collection Placement: Crystal / Mythic / A
Current RewardPool References: Legendary, Mythic
Final Mapping Status: Unresolved

The current prototype reward pools contain cross-tier references. Those references are
prototype-only and do not redefine catalog identity. Approved production Mining design
requires normal reward pools to reference the same rarity tier; final memberships and
weights remain content work.

DESIGN QUESTION DQ-IC-04:
For each of the nine legacy identifiers, decide whether it maps to one approved production
ItemId, is renamed/replaced, or is retired during the development reset. Until that
decision, no current Collection placement is a final production mapping.

DESIGN QUESTION DQ-IC-05:
Approve production reward-pool membership and weights after final ItemIds exist. Current
prototype cross-tier memberships are historical references, not production content.

SCOPE NOTE

UO-3500 changes design documentation only. It does not create ItemConfig, migrate
ContainerConfig/GemConfig/CollectionCatalog, alter runtime behavior, change UI or models,
or modify save data.
