26_LEGACY_ITEM_DISPOSITION
Status: AUTHORITATIVE DESIGN
Ticket: UO-3502 - Legacy Item Disposition Record
Supersedes: the "LEGACY PROTOTYPE MIGRATION / REFERENCE" table previously duplicated in
25_ITEM_CATALOG_DESIGN, and the legacy identifier list previously duplicated in
14_FIRST_PROTOTYPE_SPEC. Both now reference this document instead of restating it.

PURPOSE

This is the single authoritative record of what happens to every retired prototype Item
identifier with respect to the production Item Catalog (25_ITEM_CATALOG_DESIGN /
ItemConfig). It does not change runtime behavior, migrate any system, or create any
mapping. It exists so no other document needs to duplicate this table again.

AUTHORITY

Game Design -> Item Catalog Design (25_ITEM_CATALOG_DESIGN) -> Runtime ItemConfig
-> Legacy Item Disposition (this document, non-normative w.r.t. catalog identity - it
records disposition of RETIRED identifiers, it does not define production identity).

Other design documents must reference this table rather than copy it.

SCOPE

This document covers ONLY the nine prototype Item identifiers currently defined in
ReplicatedStorage/Shared/Config/GemConfig (GemConfig.Gems keys). It does not cover
ContainerType, GameEnums, or any production ItemConfig entry - those are catalog-side
concepts, not legacy identifiers.

DISPOSITION RULE (applies identically to all nine identifiers below)

Every legacy identifier in this document is:
- RETIRED
- No Production ItemId (it is not, and will never automatically become, one of the 96
  approved <Cave>_<Rarity>_<Position> ItemIds)
- No Production Catalog Slot (it occupies no Cave/Rarity/Position slot in ItemConfig)
- No Alias (no compatibility alias, synonym, or lookup redirect exists or may be created
  from this identifier to any production ItemId)
- No Automatic Mapping (no code path may infer, derive, or guess a production ItemId from
  this identifier; any future correspondence requires an explicit, separately-approved
  migration decision)
- No SaveData Migration (persisted player data referencing this identifier - e.g. via
  GemType/ItemType fields in Backpack/Inventory/Collection - is not migrated, rewritten,
  or reinterpreted by anything in this document; SaveData schema is untouched)
- Historical Reference Only (this entry exists purely to record what the identifier WAS
  and where it lived in the prototype; it authorizes nothing going forward)

Do not infer a mapping, alias, or migration for any identifier below beyond what is
explicitly stated. A later, explicitly-scoped migration ticket may revisit this - this
document does not pre-approve one.

LEGACY DISPOSITION TABLE (9 of 9 documented legacy prototype identifiers)

1. Coal
   Prototype GemConfig Rarity: Common
   Prototype CollectionCatalog Placement: Crystal / Common / A
   Prototype RewardPool References: Common
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

2. Quartz
   Prototype GemConfig Rarity: Common
   Prototype CollectionCatalog Placement: Crystal / Common / B
   Prototype RewardPool References: Common, Uncommon
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

3. Ruby
   Prototype GemConfig Rarity: Uncommon
   Prototype CollectionCatalog Placement: Crystal / Uncommon / A
   Prototype RewardPool References: Common, Uncommon, Rare
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

4. Emerald
   Prototype GemConfig Rarity: Rare
   Prototype CollectionCatalog Placement: Crystal / Rare / A
   Prototype RewardPool References: Uncommon, Rare, Epic
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

5. Sapphire
   Prototype GemConfig Rarity: Rare
   Prototype CollectionCatalog Placement: Crystal / Rare / B
   Prototype RewardPool References: Rare, Epic
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

6. Diamond
   Prototype GemConfig Rarity: Epic
   Prototype CollectionCatalog Placement: Crystal / Epic / A
   Prototype RewardPool References: Epic, Legendary
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

7. AuroraCrystal
   Prototype GemConfig Rarity: Legendary
   Prototype CollectionCatalog Placement: Crystal / Legendary / A
   Prototype RewardPool References: Legendary, Mythic
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

8. CelestialQuartz
   Prototype GemConfig Rarity: Legendary
   Prototype CollectionCatalog Placement: Crystal / Legendary / B
   Prototype RewardPool References: Legendary, Mythic
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

9. SecretCrystalItem
   Prototype GemConfig Rarity: Mythic
   Prototype CollectionCatalog Placement: Crystal / Mythic / A
   Prototype RewardPool References: Legendary, Mythic
   Disposition: RETIRED - No Production ItemId - No Production Catalog Slot - No Alias -
   No Automatic Mapping - No SaveData Migration - Historical Reference Only

COUNT CHECK: exactly 9 identifiers documented above, matching exactly the 9 keys currently
defined in GemConfig.Gems (Coal, Quartz, Ruby, Emerald, Sapphire, Diamond, AuroraCrystal,
CelestialQuartz, SecretCrystalItem). No identifier is omitted; no additional identifier is
included.

RUNTIME PRESERVATION

This document changes nothing about the prototype runtime. GemConfig, ContainerConfig
reward pools, CollectionCatalog, AnalyzeService, InventoryService, and SaveData all
continue to use these nine identifiers exactly as before. "RETIRED" describes their
production-catalog status, not their prototype runtime status - prototype runtime assets
are not removed by this document (that remains an explicit, separate future migration
ticket, not yet scheduled).

RELATION TO DQ-IC-04

DQ-IC-04 (RESOLVED, recorded in 25_ITEM_CATALOG_DESIGN) is the design decision that all
nine prototype identifiers are retired with no production mapping. This document is the
detailed, per-identifier record implementing that resolved decision - it does not reopen,
narrow, or expand DQ-IC-04.

VALIDATION SUMMARY (see UO-3502 implementation report for full detail)

- Legacy identifier count documented here: 9. GemConfig.Gems key count: 9. Match: yes.
- ItemConfig (ReplicatedStorage/Shared/Config/ItemConfig) LEGACY_ITEM_IDS table already
  lists these same 9 identifiers and asserts (ItemConfig.Validate, run at require-time)
  that none of them appear as a production ItemId. This document is consistent with, and
  does not modify, that existing runtime assertion.
- Production ItemConfig entry count: 96. Legacy identifiers found among those 96 ItemIds: 0.
- No alias table, mapping table, or migration function exists anywhere in the project
  associating any of these 9 identifiers with a production ItemId.
