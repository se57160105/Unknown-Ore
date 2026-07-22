25_ITEM_CATALOG_DESIGN
Status: AUTHORITATIVE DESIGN
Ticket: UO-3500 - Authoritative Item Catalog Design Refactor
Approved decisions recorded for UO-3501 - Runtime ItemConfig Foundation

PURPOSE

This document is the single design authority for collectible item identity and for the
complete item-to-Collection-position catalog. UO-3501 creates the runtime ItemConfig
foundation that implements this design without migrating downstream systems. Gameplay
systems consume catalog data; they do not independently define it.

AUTHORITY AND DEPENDENCY DIRECTION

Game Design
-> Item Catalog Design (this document)
-> Runtime ItemConfig
-> Legacy Migration
-> Analyze Migration (including ContainerConfig reward references and Inventory grants)
-> Collection Migration
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
- RewardPool membership: A reference owned exclusively by RewardPoolConfig. ItemConfig
  contains no membership, weight, chance, or pool fields. Membership never redefines the
  item's Cave, Rarity, CollectionRarity, or CollectionPosition.

FINALIZED CATALOG SHAPE

- Caves: Crystal, Lava, Frozen, Meteor
- Rarities per Cave: Common, Uncommon, Rare, Epic, Legendary, Mythic
- Positions per Cave/Rarity: A, B, C, D
- Items per Cave: 24
- Unique items globally: 96
- Every collectible has exactly one stable ItemId and one Collection position.
- ItemIds are globally unique; Cave/Rarity/Position tuples are globally unique.
- No gameplay system may independently define collectible identity.

UO-3501 RUNTIME ITEMCONFIG SCHEMA

Every UO-3501 production entry contains exactly:
- ItemId
- DisplayName
- Cave
- Rarity
- CollectionPosition

UO-3501 must not add Description, Tags, VisualKey, ModelKey, IconKey, SellTier,
BaseSellValue, Weight, DropChance, PoolName, or RewardPool membership.

DESIGN DECISION DQ-IC-01 - RESOLVED:
ItemConfig stores Rarity and contains no SellTier or BaseSellValue. Base sell value is
derived from Rarity through a centralized Economy configuration. Economy balancing is
not implemented by UO-3501.

Random Quality, random Value, and random per-instance gameplay stats are not part of
production catalog identity.

PROTOTYPE RUNTIME - NOT PRODUCTION DESIGN

The current runtime is preserved temporarily for compatibility and testing. These facts
describe Prototype Runtime only and are not production requirements:
- Only Crystal has current reward-pool/content implementation.
- The nine legacy prototype identifiers (full list and per-identifier disposition:
  26_LEGACY_ITEM_DISPOSITION) are not approved production ItemIds.
- CollectionCatalog is a partial prototype mapping, not the catalog authority.
- Current item models/icons/visuals are generic, placeholder, or incomplete.
- Analyze currently creates random Quality and a derived per-instance Value; neither is
  part of finalized catalog identity.
- Current ContainerConfig reward pools are prototype memberships and include cross-tier
  references.
- Before UO-3501, no runtime ItemConfig existed. UO-3501 adds only an isolated production
  foundation; current prototype consumers remain on legacy configs until later migrations.

This section records current behavior without approving it. UO-3501 creates no Analyze,
Collection, Shelf, Economy, UI, reward-pool, or save migration.

ITEM ID STATUS

DESIGN DECISION DQ-IC-02 - RESOLVED:
Every production ItemId uses <Cave>_<Rarity>_<Position>, with approved segments exactly as
shown in the 96-slot table below. Examples: Crystal_Common_A, Lava_Rare_C,
Frozen_Legendary_B, Meteor_Mythic_D.

DisplayName is never an ItemId. Production ItemIds are stable and must not be renamed
after release. NAME PENDING is the unresolved DisplayName placeholder.

DESIGN DECISION DQ-IC-03 - DEFERRED:
All 96 DisplayNames remain NAME PENDING. Final DisplayNames, models, icons, visual keys,
and visual concepts belong to UO-3600 - Item Identity Design. UO-3501 invents or stores
none of those fields.

COMPLETE 96-SLOT CATALOG

## CRYSTAL
Crystal_Common_A - NAME PENDING
Crystal_Common_B - NAME PENDING
Crystal_Common_C - NAME PENDING
Crystal_Common_D - NAME PENDING
Crystal_Uncommon_A - NAME PENDING
Crystal_Uncommon_B - NAME PENDING
Crystal_Uncommon_C - NAME PENDING
Crystal_Uncommon_D - NAME PENDING
Crystal_Rare_A - NAME PENDING
Crystal_Rare_B - NAME PENDING
Crystal_Rare_C - NAME PENDING
Crystal_Rare_D - NAME PENDING
Crystal_Epic_A - NAME PENDING
Crystal_Epic_B - NAME PENDING
Crystal_Epic_C - NAME PENDING
Crystal_Epic_D - NAME PENDING
Crystal_Legendary_A - NAME PENDING
Crystal_Legendary_B - NAME PENDING
Crystal_Legendary_C - NAME PENDING
Crystal_Legendary_D - NAME PENDING
Crystal_Mythic_A - NAME PENDING
Crystal_Mythic_B - NAME PENDING
Crystal_Mythic_C - NAME PENDING
Crystal_Mythic_D - NAME PENDING

## LAVA
Lava_Common_A - NAME PENDING
Lava_Common_B - NAME PENDING
Lava_Common_C - NAME PENDING
Lava_Common_D - NAME PENDING
Lava_Uncommon_A - NAME PENDING
Lava_Uncommon_B - NAME PENDING
Lava_Uncommon_C - NAME PENDING
Lava_Uncommon_D - NAME PENDING
Lava_Rare_A - NAME PENDING
Lava_Rare_B - NAME PENDING
Lava_Rare_C - NAME PENDING
Lava_Rare_D - NAME PENDING
Lava_Epic_A - NAME PENDING
Lava_Epic_B - NAME PENDING
Lava_Epic_C - NAME PENDING
Lava_Epic_D - NAME PENDING
Lava_Legendary_A - NAME PENDING
Lava_Legendary_B - NAME PENDING
Lava_Legendary_C - NAME PENDING
Lava_Legendary_D - NAME PENDING
Lava_Mythic_A - NAME PENDING
Lava_Mythic_B - NAME PENDING
Lava_Mythic_C - NAME PENDING
Lava_Mythic_D - NAME PENDING

## FROZEN
Frozen_Common_A - NAME PENDING
Frozen_Common_B - NAME PENDING
Frozen_Common_C - NAME PENDING
Frozen_Common_D - NAME PENDING
Frozen_Uncommon_A - NAME PENDING
Frozen_Uncommon_B - NAME PENDING
Frozen_Uncommon_C - NAME PENDING
Frozen_Uncommon_D - NAME PENDING
Frozen_Rare_A - NAME PENDING
Frozen_Rare_B - NAME PENDING
Frozen_Rare_C - NAME PENDING
Frozen_Rare_D - NAME PENDING
Frozen_Epic_A - NAME PENDING
Frozen_Epic_B - NAME PENDING
Frozen_Epic_C - NAME PENDING
Frozen_Epic_D - NAME PENDING
Frozen_Legendary_A - NAME PENDING
Frozen_Legendary_B - NAME PENDING
Frozen_Legendary_C - NAME PENDING
Frozen_Legendary_D - NAME PENDING
Frozen_Mythic_A - NAME PENDING
Frozen_Mythic_B - NAME PENDING
Frozen_Mythic_C - NAME PENDING
Frozen_Mythic_D - NAME PENDING

## METEOR
Meteor_Common_A - NAME PENDING
Meteor_Common_B - NAME PENDING
Meteor_Common_C - NAME PENDING
Meteor_Common_D - NAME PENDING
Meteor_Uncommon_A - NAME PENDING
Meteor_Uncommon_B - NAME PENDING
Meteor_Uncommon_C - NAME PENDING
Meteor_Uncommon_D - NAME PENDING
Meteor_Rare_A - NAME PENDING
Meteor_Rare_B - NAME PENDING
Meteor_Rare_C - NAME PENDING
Meteor_Rare_D - NAME PENDING
Meteor_Epic_A - NAME PENDING
Meteor_Epic_B - NAME PENDING
Meteor_Epic_C - NAME PENDING
Meteor_Epic_D - NAME PENDING
Meteor_Legendary_A - NAME PENDING
Meteor_Legendary_B - NAME PENDING
Meteor_Legendary_C - NAME PENDING
Meteor_Legendary_D - NAME PENDING
Meteor_Mythic_A - NAME PENDING
Meteor_Mythic_B - NAME PENDING
Meteor_Mythic_C - NAME PENDING
Meteor_Mythic_D - NAME PENDING

CATALOG INVARIANTS

- Exactly 4 Caves x 6 Rarities x 4 Positions = 96 catalog entries.
- No duplicate ItemId.
- No duplicate Cave/Rarity/CollectionPosition tuple.
- Exactly one Collection position per ItemId.
- Collection completion derives its totals from the authoritative runtime catalog.
- Other design documents must reference this table rather than copy it.

LEGACY PROTOTYPE MIGRATION / REFERENCE

[UO-3502] The full legacy disposition table (all nine prototype identifiers, their
prototype rarity/placement/reward-pool facts, and their explicit per-identifier
disposition) now lives exclusively in 26_LEGACY_ITEM_DISPOSITION - the single authoritative
record. This document no longer duplicates that table; reference 26_LEGACY_ITEM_DISPOSITION
for the complete per-identifier detail.

Summary only: all nine prototype identifiers (Coal, Quartz, Ruby, Emerald, Sapphire,
Diamond, AuroraCrystal, CelestialQuartz, SecretCrystalItem) are RETIRED, receive no
production ItemId, occupy no production catalog slot, and may remain in prototype runtime
until a later, explicitly-scoped migration/removal ticket.

The current prototype reward pools contain cross-tier references. Those references are
prototype-only and do not redefine catalog identity. Approved production Mining design
requires normal reward pools to reference the same rarity tier; final memberships and
weights remain content work.

DESIGN DECISION DQ-IC-04 - RESOLVED:
All nine prototype identifiers are retired. They receive no production mapping, occupy no
production catalog slot, and must not appear in production ItemConfig. Prototype test data
requires no production migration. Full legacy runtime removal is deferred to a later
explicit migration ticket. See 26_LEGACY_ITEM_DISPOSITION for the authoritative per-
identifier record implementing this decision.

DESIGN DECISION DQ-IC-05 - RESOLVED [UPDATED UO-3505-R1, stale equal-weight statement
corrected]:
RewardPool composition and weights belong exclusively to RewardPoolConfig. ItemConfig
contains no Weight, DropChance, PoolName, or membership fields. Production pools contain
the four same-Cave/same-Rarity items, one per Collection Position (A, B, C, D). Selection
is NOT equal-weight/uniform: RewardPoolConfig resolves Cave + Container Tier -> Rarity,
rolls a Position using the approved Analyze position distribution (18_CONTENT_DATABASE
NORMAL POSITION DISTRIBUTION: A=40, B=30, C=20, D=10 by default; higher positions are
strictly less likely than lower positions), then resolves the production ItemId for that
Cave/Rarity/Position. UO-3501 does not create or migrate RewardPoolConfig and does not
alter prototype pools. UO-3505/UO-3505-R1 implement RewardPoolConfig itself but do not
migrate Analyze to consume it.

SCOPE NOTE

UO-3500 created this design authority. UO-3501 creates only the validated, read-only
ItemConfig foundation with 96 NAME PENDING entries. It does not migrate or remove
ContainerConfig/GemConfig/CollectionCatalog, alter reward pools, change gameplay, UI,
models, economy balancing, or save data.
