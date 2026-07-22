27_RUNTIME_CATALOG_AUDIT
Status: AUDIT RECORD (DOCUMENTATION ONLY)
Ticket: UO-3503 - Runtime Catalog Audit

PURPOSE

This is a factual, code-verified audit of every current runtime system that produces,
stores, or displays Item data, and how each relates to the production Item Catalog
(25_ITEM_CATALOG_DESIGN / ItemConfig) versus the prototype architecture (GemConfig /
ContainerConfig / CollectionCatalog). It changes no runtime behavior. It exists to give
any future migration ticket (Analyze Migration, Collection Migration, Shelf, Economy, UI)
a single accurate starting map instead of re-discovering this dependency graph each time.

METHOD

Every finding below was verified directly against the current Roblox Studio project
(source read, or project-wide text search for `require(...ItemConfig)` /
`require(...GemConfig)` / `require(...ContainerConfig)` / `require(...CollectionCatalog)`),
not inferred from design documents alone. Where a design document's stated intent differs
from the verified runtime behavior, both are stated separately and the difference is noted.

TICKET NUMBER SYNCHRONIZATION

UO-3503 is the completed Runtime Catalog Audit, an explicitly documentation-only,
no-runtime-change ticket. 99_ROADMAP and 13_IMPLEMENTATION_PLAN now identify the future
runtime migration with the placeholder ticket number TBD. This audit does not create,
define, or perform that future migration ticket.

================================================================================
SYSTEM-BY-SYSTEM AUDIT
================================================================================

1. ItemConfig (ReplicatedStorage/Shared/Config/ItemConfig)
---------------------------------------------------------
Purpose: The production Item Catalog foundation - 96 frozen, validated identity/
classification records (ItemId, DisplayName, Cave, Rarity, CollectionPosition).
Current data source(s): Self-contained; requires only GameEnums.
Prototype dependencies: None.
Production dependencies: Implements 25_ITEM_CATALOG_DESIGN directly; is itself the
production dependency every other system will eventually consume.
Legacy identifier usage: None present. Its own LEGACY_ITEM_IDS table + Validate()
assertion actively guarantees none of the 9 legacy identifiers can appear in it.
Runtime ownership: Read-only config module; owns nothing mutable (fully frozen).
Current migration status: PRODUCTION READY, AND LIVE IN GAMEPLAY [UPDATED THROUGH
UO-3508]:
  ItemConfig
  ├─ consumed by DataService for production ItemId validation (section 6; live since UO-3504)
  ├─ consumed by RewardPoolConfig for production reward-pool construction (section 1B)
  ├─ consumed by AnalyzeService for ItemId validation at Submit/Claim (section 4; live since
  │  UO-3506)
  ├─ consumed by CollectionService for production discovery and summary calculation (live
  │  since UO-3507)
  ├─ consumed by InventoryUI for production Item presentation (live since UO-3506.5)
  └─ consumed by CollectionUI for production catalog presentation and progress calculation
     (live since UO-3508)
These six runtime consumers were verified directly. Analyze migration is COMPLETE
(UO-3506/UO-3506-R1), Collection discovery/summary is production-backed (UO-3507), and
CollectionUI now renders production ItemConfig identities (UO-3508). DisplayName
remains NAME PENDING for all 96 entries (DQ-IC-03, deferred to UO-3600) - this does not
block structural consumption, only presentation.

1B. RewardPoolConfig (ReplicatedStorage/Shared/Config/RewardPoolConfig)
--------------------------------------------------------------------------
Purpose: The production reward-SELECTION foundation (UO-3505, UO-3505-R1) - maps each of
the 24 Cave/ContainerTier pools to their four same-Cave/same-Rarity production ItemIds,
addressable by Collection Position (A/B/C/D), and resolves a rolled Position to its
production ItemId per the approved Analyze position distribution (18_CONTENT_DATABASE
NORMAL POSITION DISTRIBUTION: A=40, B=30, C=20, D=10 by default; not equal-weight).
Current data source(s): Self-contained; built directly from ItemConfig.GetAll() and
GameEnums (Cave/Rarity/ContainerTier/Position order + ContainerTierToRarity mapping).
Prototype dependencies: None.
Production dependencies: Requires ItemConfig directly (reads every definition to build and
validate pool membership and Position placement); is itself the production dependency
AnalyzeService now consumes (see section 4).
Legacy identifier usage: None present; its own Validate() rejects any ItemId that does not
resolve through ItemConfig, and rejects cross-cave/cross-tier/position-mismatched entries.
Runtime ownership: Read-only config module; owns nothing mutable (fully frozen, including
the default position-distribution table).
Current migration status: PRODUCTION READY, AND LIVE IN GAMEPLAY [UPDATED UO-3506]. Verified
via project-wide search: AnalyzeService.HandleBatchRequest calls RewardPoolConfig.HasPool/
SelectRandomItemId directly at Submit (section 4). AnalyzeService no longer reads
ContainerConfig.RewardPools or GemConfig at all - the Analyze migration recommended by this
audit's RECOMMENDED MIGRATION ORDER step 4 is now COMPLETE.

2. GemConfig (ReplicatedStorage/Shared/Config/GemConfig)
---------------------------------------------------------
Purpose: PROTOTYPE RUNTIME item metadata (DisplayName, Rarity, BaseValue, BuffType/
BuffAmount) for the 9 legacy identifiers.
Current data source(s): Self-contained static table (GemConfig.Gems, GemConfig.RarityColors).
Prototype dependencies: Is itself the prototype dependency.
Production dependencies: None.
Legacy identifier usage: Defines all 9 (Coal, Quartz, Ruby, Emerald, Sapphire, Diamond,
AuroraCrystal, CelestialQuartz, SecretCrystalItem) - this IS the source of that list
(cross-checked against 26_LEGACY_ITEM_DISPOSITION and ItemConfig's own LEGACY_ITEM_IDS;
all three agree, 9 for 9).
Runtime ownership: Read-only config; no mutable state.
Current migration status: REQUIRES MIGRATION for its remaining consumers [UPDATED
THROUGH UO-3508]. AnalyzeService, ClientBootstrap, and CollectionUI no longer require
GemConfig. InventoryUI still requires it only for the supported prototype Item branch while
using ItemConfig for production Items. Verified consumers: DisplayService (server, prototype
Shelf Buff resolution only) plus InventoryUI, ShopUI, and ShelfUI (client prototype
presentation) - 4 files total. CollectionUI was removed from this dependency in UO-3508 and
now derives its catalog, identity, grouping, and discovered presentation from ItemConfig.

3. ContainerConfig (ReplicatedStorage/Shared/Config/ContainerConfig)
---------------------------------------------------------------------
Purpose: Unknown Container identity (theme/rarity naming) and PROTOTYPE reward pools
(RewardPools: ContainerRarity -> weighted list of GemConfig ItemTypes).
Current data source(s): Self-contained static table. Only the Crystal theme is defined;
Lava/Frozen/Meteor have no entry at all.
Prototype dependencies: RewardPools reference GemConfig ItemTypes by string literal.
Production dependencies: None currently; DQ-IC-05 (RESOLVED) designates RewardPoolConfig -
implemented, see section 1B - as the sole owner of pool membership/weights.
ContainerConfig's RewardPools table is now PURE DEAD DATA for Analyze [UPDATED UO-3506]:
AnalyzeService no longer reads it at all (RewardPoolConfig fully replaced it as the reward
source). The RewardPools table itself has not been deleted ("Legacy removal"/"Prototype
cleanup beyond what is required" was explicitly out of scope for UO-3506/UO-3506-R1) but is
now unreachable from any runtime code path.
Legacy identifier usage: All prototype RewardPools entries reference legacy GemConfig
ItemTypes (verified: every `GemType = "..."` in ContainerConfig.Themes.Crystal.RewardPools
is one of the 9 legacy identifiers) - these entries are dead data (see above), not a live
legacy-identifier risk.
Runtime ownership: Read-only config; `ContainerConfig.RollRarity()` is present but is
DEAD CODE for Mining (Mining rarity now rolls via BalanceConfig.Mining.BaseRarityRates,
per UO-1005) - flagged under Hidden Coupling below since a future reader could mistake it
for live logic.
Current migration status: REWARD OWNERSHIP MIGRATION COMPLETE [UPDATED UO-3506]. Reward pool
membership now lives exclusively in RewardPoolConfig per DQ-IC-05; AnalyzeService rolls
production ItemIds from it, never from ContainerConfig.RewardPools. Verified consumers:
MiningService (server, container generation only - no reward identity); AnalyzeService
(display-only now, via GetDisplayName - see section 4), ClientBootstrap, AnalyzeMachineUI,
BackpackUI, LabStorageUI, CollectionUI (client, display-only for Container/theme text -
these do not touch Item identity).

4. AnalyzeService (ServerScriptService/Server/Services/AnalyzeService)
-------------------------------------------------------------------------
Purpose: Batch submit, per-slot timers, reward resolution, Claim, and the sole creation
point for Actual Item Instances.
[UPDATED UO-3506/UO-3506-R1 - PRODUCTION MIGRATION COMPLETE. Previous entry below described
the PROTOTYPE implementation this audit found in UO-3503; that implementation has since
been replaced.]
Current data source(s): RewardPoolConfig (Cave/ContainerTier pool + Position roll),
ItemConfig (ItemId validation/lookup), DataService profile state, InventoryService
capacity/grant validation, AnalyzeMachine/ActiveBuff values, ContainerConfig (display-only,
GetDisplayName), and Bootstrap-wired Analyze remotes.
Prototype dependencies: NONE. `ContainerConfig.RewardPools` and `GemConfig` are no longer
required or read anywhere in this module.
Production dependencies: ItemConfig identity + RewardPoolConfig reward selection (both now
live consumers - see sections 1 and 1B), the clean Actual Item schema/validator (DataService,
section 6), and the public replicated projection that redacts the persisted ResultItemId
until Claim (DataService.PUBLIC_PROFILE_SCHEMA.AnalyzeSlots omits it entirely).
Legacy identifier usage: NONE. Every Item Instance this service creates is production-shaped
(ItemUID/ItemId/ObtainedAt/Location/IsLocked) - no ItemType, Quality, or Value fields exist.
Runtime ownership: Owns persisted `data.AnalyzeSlots`, including the server-only
`ResultItemId` field on each slot. There is no longer any ephemeral `pendingItems` table -
ResultItemId is rolled and LOCKED once, at Submit (HandleBatchRequest), directly on the
persisted slot, matching 04_ANALYZE_DESIGN's RESULT TIMING/AUTHORITATIVE RESULT IDENTITY and
the IMPLEMENTATION NOTES "RESULT IS PERSISTED AT SUBMIT, AND SERVER-ONLY"/"NEVER RE-ROLL"
rules exactly. resolveSlot (timer completion), HandleClaim (manual Claim), and ResumePlayer
(reconnect/restart) all read the same persisted ResultItemId back - none of them roll or
re-roll. An invalid container (Theme/Rarity that cannot resolve to a Cave/ContainerTier with
an eligible RewardPoolConfig pool) is rejected at Submit WITHOUT being removed from the
Backpack (validated via a read-only InventoryService.GetContainer peek before the mutating
InventoryService.RemoveContainer call). HandleClaim also now writes a production Collection
entry keyed by ItemId (Discovered/FirstFoundAt/TotalFound) - only after InventoryService.
AddItem confirms the grant - and returns the resulting new-discovery state (IsNew) in the
AnalyzeResult payload.
Current migration status: PRODUCTION MIGRATION COMPLETE for reward identity, Item creation,
result persistence/locking, and Collection recording. This audit's own RECOMMENDED MIGRATION
ORDER step 4 ("ANALYZE VERTICAL SLICE") is now done. 17_SAVE_SYSTEM marks DQ-2 through DQ-8
as historical and removed from the critical path under the approved development reset
policy; that remains true and unaffected by this migration.

5. InventoryService (ServerScriptService/Server/Services/InventoryService)
-------------------------------------------------------------------------
Purpose: Owns the two separate storages - Mining Backpack (Containers) and Inventory
(Item Instances) - Add/Remove/Get/HasCapacity for both.
Current data source(s): DataService session data, DataService.IsValidItem, and the shared
schema-aware DataService.GetItemInstanceKey helper.
Prototype dependencies: No direct config dependency. Existing prototype callers continue to
use ItemId as their unique instance key.
Production dependencies: UO-3504 added production validation and ItemUID-based instance
identity without migrating any producer or downstream consumer.
Legacy identifier usage: None directly; it does not resolve ItemType against GemConfig.
Runtime ownership: Owns `data.Backpack.Containers` and `data.Inventory.Items` mutation.
Current migration status: PRODUCTION FOUNDATION READY for Inventory validation, insertion,
deduplication, lookup, and removal. Analyze, Shelf, Economy, and UI consumer migration
remains separate.

6. DataService (ServerScriptService/Server/Data/DataService) - Item-relevant surface only
-------------------------------------------------------------------------------------------
Purpose (Item-relevant slice): shared validation, deterministic load-time
sanitization, legacy schema conversion, profile save, and client replication.
Current data source(s): Current profile/session data, DefaultData, BalanceConfig,
ItemConfig, and the DataUpdated RemoteEvent.
Current compatibility:
- `IsValidItem` accepts the unchanged prototype schema or the approved production schema.
- Production ItemId is validated through ItemConfig; production instance identity is ItemUID.
- Production validation and sanitization share the same exact five-field allowlist, so no
  accepted production field is silently discarded on load.
- `sanitizeItemList` deterministically rebuilds the matching schema and deduplicates by its
  instance key.
- The live v3-to-v4 conversion still maps GemType to the legacy prototype Item shape.
- `Replicate` sends an explicit fail-closed public allowlist projection through DataUpdated;
  raw `session.Data` is never passed to the client, and migration metadata is excluded.
Production dependencies still outstanding: the clean future SchemaVersion and the coordinated
Analyze/Collection/Shelf/Economy/UI migrations. [ADDED UO-3506-R2, SIMPLIFIED UO-3506-R4] Also
now requires GameEnums and RewardPoolConfig, used exclusively by the new `reconcileAnalyzeSlots`
(see below) - a one-time, idempotent, load-time reconciliation for Analyze slots persisted
before `ResultItemId` existed (pre-UO-3506-R1). For each such slot: if its Theme/Rarity still
resolve to a valid Cave/ContainerTier with an eligible RewardPoolConfig pool, it is rolled
EXACTLY ONCE via `RewardPoolConfig.SelectRandomItemId` and the result is persisted directly
onto the slot - after that, the slot is indistinguishable from one AnalyzeService itself
created at Submit, and every later Ready/Claim/retry/reconnect/restart path reads the same
persisted value back and never re-rolls it. A slot already carrying `ResultItemId` is left
completely untouched (the reconciliation is a no-op for it on every subsequent load - true
idempotency, not just "safe to repeat"). [SIMPLIFIED UO-3506-R4] A slot whose Theme/Rarity
CANNOT resolve is simply RETAINED EXACTLY AS-IS - quarantined, never deleted, never rerolled,
and never given an invented Cave/Rarity/ItemId/Container - with a clear server warning logged
(17_SAVE_SYSTEM DQ-8: quarantine, never silently delete). This slot CONTINUES OCCUPYING AN
ANALYZE SLOT indefinitely; NO AUTOMATIC CONTAINER REFUND EXISTS, and manual or future,
explicitly approved disposition is still required to actually reclaim it. UO-3506-R2/R3
previously attempted a Backpack/LabStorage Container refund gated on re-validating against
this same Analyze mapping; that gate could never pass in practice - a slot only reached that
branch by first failing to resolve its OWN Theme/Rarity against the identical mapping, so the
refund's re-check was tautologically unreachable - and has been removed as dead code
(UO-3506-R4). No `DataVersion` bump was needed (matches the UO-3504 precedent of adding a
compatible field without a schema-version transition).
Legacy identifier usage: The prototype validator and v3-to-v4 writer continue to support
legacy ItemType-shaped Inventory records during the compatibility window.
Runtime ownership: Owns profile load, migration, sanitization, save, and replication.
[ADDED UO-3506-R2, SIMPLIFIED UO-3506-R4] Also owns the one-time `ResultItemId` reconciliation
for legacy AnalyzeSlots and the retain-and-warn quarantine of unresolvable ones - no Container
refund path exists; it no longer touches Backpack or LabStorage at all.
Current migration status: PRODUCTION FOUNDATION READY for dual-schema Item validation,
sanitization, instance identity, newer-version refusal, and public projection. LEGACY ANALYZE
JOB RECOVERY IS PARTIAL, NOT COMPLETE (UO-3506-R4): a resolvable legacy slot is recovered
fully (one persisted ResultItemId, idempotent); an unresolvable slot is only QUARANTINED
(retained + warned), still occupies an Analyze slot, and has no automatic disposition -
reclaiming it requires a future, explicitly approved mechanism that does not exist yet.
Runtime producer/consumer migration remains separate.

7. CollectionCatalog (ReplicatedStorage/Shared/Config/CollectionCatalog)
---------------------------------------------------------------------------
Purpose: Retained prototype mapping of the 24-slot-per-cave/6-rarity/4-position grid
shape onto the 9 legacy GemConfig items. It no longer supplies CollectionUI at runtime.
Current data source(s): Hand-authored `CAVE_RARITY_ITEMS` table (GemConfig ItemTypes
placed into Cave/Rarity/Position slots by array order - not derived from any other config).
Prototype dependencies: Entirely prototype - every non-empty slot references a GemConfig
ItemType.
Production dependencies: None. Its own header comment still states ItemConfig "has NOT been
built yet" - factually stale because ItemConfig was created in UO-3501.
Legacy identifier usage: All 9 legacy identifiers appear here, each hand-placed into one
Cave/Rarity/Position slot.
Runtime ownership: None. Project-wide source search finds no current `require` consumer;
the module is inert retained prototype data.
Current migration status: RETIRED FROM THE COLLECTION UI PATH (UO-3508). CollectionUI now
groups `ItemConfig.GetAll()` directly by Cave/Rarity/CollectionPosition. The module remains
present as unused prototype data; deleting or rewriting it was not part of UO-3508.

8. EconomyService (ServerScriptService/Server/Services/EconomyService)
---------------------------------------------------------------------------
Purpose: Sell Inventory items for Coins.
Current data source(s): DataService profile state, InventoryService mutation, the stored
`item.Value` field, and ActiveBuffs sell multipliers.
Prototype dependencies: `getCoinsFor` multiplies `item.Value` (the prototype's random,
per-instance rolled field) by Shelf-buff multipliers - CONFLICTS with the approved
DQ-IC-01 decision ("Base sell value is derived from Rarity through a centralized Economy
configuration" - not from a stored per-item Value). No such centralized Economy config
currently exists anywhere in the project (verified: no `EconomyConfig`/similar module
found).
Production dependencies: None.
Legacy identifier usage: Indirect only, via `item.ItemType` used as the `data.Collection`
key in `creditSale` for a PROTOTYPE item [UPDATED UO-3506-R1: `creditSale`/`getCollectionKey`
are now schema-aware - a production item (has ItemUID) is keyed by `item.ItemId` instead,
matching AnalyzeService's own production Collection writes (section 4); the prototype path
is unchanged].
Runtime ownership: Owns Coin crediting and Collection `TotalSold` increment.
Current migration status: PRODUCTION ITEMS ARE NOT SELLABLE (BY DESIGN) [UPDATED UO-3506-R2,
HARDENED UO-3506-R3], FULL MIGRATION STILL REQUIRED. A new `isProductionItem` identity gate
(`item.ItemUID ~= nil`) rejects a production Item BEFORE the location/favorite checks, in both
`HandleSellGem` paths: Sell All simply skips it, and single Sell [HARDENED UO-3506-R3] now
peeks the item read-only via `InventoryService.GetItem` FIRST and evaluates every
eligibility check (production-shaped, favorite, location) against that peek - an ineligible
item is rejected WITHOUT `RemoveItem` ever being called, replacing the previous
remove-then-restore pattern entirely (there is nothing to restore because nothing was ever
removed). `creditSale` is therefore still UNREACHABLE for any production Item - selling one
for the literal `getCoinsFor` fallback (0 Coins) is explicitly rejected as NOT ACCEPTED
BEHAVIOR, superseding UO-3506-R1's "sells for 0 Coins" compatibility fix (see Hidden Coupling
#3, updated). This is a deliberate, temporary restriction, not a bug: production Items will
become sellable only once the centralized Rarity-based Economy config (DQ-IC-01) exists.
Prototype-item selling is completely unaffected (`isProductionItem` is always false for
them). Also has its own pre-existing gap unrelated to the catalog migration: `isNearShop`
FAILS OPEN (returns `true`, allowing selling from anywhere) if the Shop part is missing,
unlike the fail-closed pattern already established in AnalyzeService/DepositService/
WithdrawService. This is a real, separate hardening gap - not introduced by this audit and
not fixed here.

9. DisplayService (ServerScriptService/Server/Services/DisplayService)
---------------------------------------------------------------------------
Purpose: Shelf placement/removal and Shelf-Buff recalculation.
Current data source(s): DataService profile state, InventoryService mutation, GemConfig.Gems
(BuffType/BuffAmount/DisplayName/RarityColors), and the physical Shelf model.
Prototype dependencies: `recalculateBuffs` resolves the active Shelf Buff DIRECTLY from
`GemConfig.Gems[item.ItemType].BuffType` - a hard per-legacy-item lookup. Design/18
(CONTENT_DATABASE) states the approved production design instead resolves "Gameplay Buff
values...from Cave identity + rarity scale + position modifier through config/resolvers" -
a fundamentally different resolution strategy (formula-based, not per-item-table-based).
No such resolver currently exists.
Production dependencies: None.
Legacy identifier usage: Every buff calculation depends on `item.ItemType` being a
GemConfig key - unaffected for prototype items; production items simply resolve no buff
(see Hidden Coupling #2 - still an accepted gap, Buff system changes are out of scope).
Runtime ownership: Owns `data.Shelves[1].Item` and `data.ActiveBuffs` recalculation.
Current migration status: CRITICAL SCHEMA-CORRUPTION BUG FIXED [UO-3506-R1], FULL BUFF
MIGRATION STILL REQUIRED. `HandlePlace`/`HandleRemove` previously wrote `item.CurrentLocation`
directly onto EVERY item unconditionally - for a production item (ItemUID present), this
added a field outside DataService's PRODUCTION_ITEM_FIELDS allowlist, so `DataService.
IsValidItem` would reject it forever afterward and `InventoryService.AddItem` inside
`HandleRemove` would then ALWAYS fail, permanently stranding the item on the Shelf with no
way to remove it (destructive mishandling, not a crash). Fixed via a schema-aware
`setItemLocation` helper: a production item now gets `item.Location = "INVENTORY"/"SHELF"`
(a field the schema already accepted); a prototype item is completely unaffected. Buff
resolution itself (`recalculateBuffs`) is unchanged and still resolves no buff for a
production item on the Shelf - the formula-based production resolver required by Design/18
remains a separate, out-of-scope migration. Also fails OPEN on a missing Shelf part (same
pattern/gap as EconomyService, not fixed here) and has an already-documented
single-shared-physical-model limitation for multiplayer (pre-existing, unrelated to
catalog migration).

10. SaveData / DefaultData (ServerScriptService/Server/Data/DefaultData) - Item-relevant fields
---------------------------------------------------------------------------------------------------
Purpose: Persisted schema for `Inventory.Items`, `Collection`,
`AnalyzeSlots`, and `Shelves[].Item`.
Current data source(s): DefaultData supplies empty/default groups; AnalyzeService and the
storage/display/economy services populate them; DataService saves the complete session.
Prototype dependencies: Inventory/Shelf Items use legacy ItemType plus Quality/Value;
Collection is keyed by legacy ItemType and stores BestQuality; AnalyzeSlots do not persist
the locked ResultItemId required by production design.
Production dependencies: A clean versioned Item/Collection/Analyze/Shelf schema, validation,
redacted replication, and persisted hidden-result support.
Legacy identifier usage: The current persisted Item and Collection identity space is the
legacy identifier space.
Runtime ownership: Data shape is defined by DefaultData and writers; DataService owns
persistence.
Current migration status: REQUIRES MIGRATION. The approved development reset means old
prototype profiles are not converted, but it does NOT remove the explicit work to define,
validate, sanitize, save, and safely replicate the clean production schema.

11. UI Systems (client) - InventoryUI, ShopUI, ShelfUI, CollectionUI, ClientBootstrap (AnalyzeResult reveal)
-------------------------------------------------------------------------------------------------------------
Purpose: Display Inventory/Shelf/Collection/reveal-notification content.
Current data source(s) [UPDATED THROUGH UO-3508]: InventoryUI uses ItemConfig for the
production Item branch and GemConfig for the supported prototype branch. ShopUI and ShelfUI
still use GemConfig for prototype presentation and contain the previously documented minimal
production safety gates. CollectionUI now requires ItemConfig and derives its full catalog,
Cave/Rarity/Position grouping, discovered presentation, and progress denominators directly
from it; it no longer requires GemConfig or CollectionCatalog. ClientBootstrap's Analyze
reveal uses production metadata sent in AnalyzeResult plus ContainerConfig rarity colors.
All panels consume the replicated DataUpdated snapshot.
Prototype dependencies: InventoryUI/ShopUI/ShelfUI retain prototype display branches.
CollectionUI has no prototype catalog or legacy ItemType dependency; legacy Collection keys
in the replicated map are ignored because discovery is validated through ItemConfig.Get.
Production dependencies: ItemConfig-backed presentation is LIVE for ClientBootstrap reveal,
InventoryUI, and CollectionUI. ShopUI/ShelfUI full production presentation remains pending.
The server public projection supplies Collection discovery records; final Item display names/
visuals remain deferred to UO-3600.
Legacy identifier usage: Indirect through prototype Item records in InventoryUI/ShopUI/
ShelfUI only.
Runtime ownership: Render/request state only; clients do not choose rewards or payouts.
Current migration status: ANALYZE REVEAL HANDLER MIGRATED (UO-3506-R1); MINIMAL SAFETY FIXES
APPLIED to ShopUI/ShelfUI (UO-3506-R1): both now send a schema-aware instance key
(`item.ItemUID or item.ItemId`) instead of always `item.ItemId` for RequestSellGem/
RequestPlaceOnShelf - sending `item.ItemId` for a production item would never match anything
server-side (ItemId is a shared catalog reference, not a unique instance id), silently
failing the sell/place action. ShelfUI also recognizes a production item's `Location` field
for its placeable-location gate (previously always false, since production items have no
`CurrentLocation`). [UPDATED UO-3506-R2] ShopUI's sellable gate now ALSO rejects any
production item outright (`isProductionItem` checked before the Location/Favorite checks) -
not merely "Location-aware", but CLEARLY DISABLED: the Sell button is greyed out
(COLOR_ROW_LOCKED/COLOR_ACCENT_OFF, `Active = false`, same treatment as a favorited or
Shelf-displayed item) and the row shows "Not sellable yet" so the player is never invited to
attempt a sell that would silently fail server-side. This is not a UI redesign - it reuses
the existing disabled-row pattern for one more reason. InventoryUI production presentation
was completed in UO-3506.5, and CollectionUI production catalog/discovery/progress presentation
was completed in UO-3508. Full ShopUI/ShelfUI production presentation remains pending.

12. Bootstrap, RemoteEvent, and replication flows
-------------------------------------------------
Purpose: Connect client intent to authoritative services and distribute resulting data.
Current flows:
- AnalyzeMachineUI -> RequestAnalyzeBatch/ClaimAnalyzeResult -> server Bootstrap ->
  AnalyzeService.
- AnalyzeService -> AnalyzeResult -> ClientBootstrap after a successful Claim.
- ShopUI -> RequestSellGem -> Bootstrap -> EconomyService.
- ShelfUI -> RequestPlaceOnShelf/RequestRemoveFromShelf -> Bootstrap -> DisplayService.
- DataService.Replicate -> explicit public allowlist projection -> DataUpdated ->
  ClientBootstrap -> every panel.
Trust boundary: Clients send container/item/slot identifiers or action intent only. Server
services resolve the player's session, ownership, location, reward, mutation, and payout.
No current client-authoritative Item metadata path was found.
Current migration status: Bootstrap's intent routing remains structurally usable.
UO-3504 now provides the fail-closed DataUpdated public projection required before a hidden
ResultItemId is persisted. AnalyzeResult and affected UI payload contracts still require
coordinated compatibility during the catalog transition.
Note: AnalyzeMachineUI/BackpackUI/LabStorageUI use ContainerConfig only for container/theme
presentation and do not consume Actual Item identity.

================================================================================
RUNTIME DEPENDENCY GRAPH
================================================================================

ItemConfig <- GameEnums only (ItemConfig's own dependency). Runtime consumers
[UPDATED THROUGH UO-3508]:
  ItemConfig
  ├─ DataService (production ItemId validation)
  ├─ RewardPoolConfig (production reward-pool construction)
  ├─ AnalyzeService (Submit/Claim ItemId validation)
  ├─ CollectionService (production discovery and summaries)
  ├─ InventoryUI (production Item presentation)
  └─ CollectionUI (production catalog, discovery presentation, and progress)
AnalyzeService now requires ItemConfig AND RewardPoolConfig directly - production Item
identity flows through Analyze into every player's Inventory. GemConfig/ContainerConfig
RewardPools are no longer read by any gameplay-reward service.

RewardPoolConfig <- ItemConfig, GameEnums (RewardPoolConfig's own dependencies - it is
itself one of the three ItemConfig consumers noted above). LIVE GAMEPLAY CONSUMER
[UPDATED UO-3506]: AnalyzeService.HandleBatchRequest calls RewardPoolConfig.HasPool/
SelectRandomItemId at Submit (section 4) - the migration this line previously marked
PENDING is now COMPLETE.

GemConfig <- DisplayService (server, prototype Shelf Buff resolution only - AnalyzeService
no longer requires GemConfig as of UO-3506)
           <- InventoryUI, ShopUI, ShelfUI (client prototype branches - ClientBootstrap and
              CollectionUI no longer require GemConfig)

ContainerConfig <- AnalyzeService (display-only, GetDisplayName - RewardPools no longer read
                    as of UO-3506), MiningService (server)
                 <- ClientBootstrap, AnalyzeMachineUI, BackpackUI, LabStorageUI,
                    CollectionUI (rarity/container presentation helpers only)

CollectionCatalog <- no current runtime consumers (retained inert prototype module).

Bootstrap routes RequestAnalyzeBatch/ClaimAnalyzeResult -> AnalyzeService,
RequestSellGem -> EconomyService, and RequestPlaceOnShelf/RequestRemoveFromShelf ->
DisplayService.

AnalyzeService [UPDATED UO-3506/UO-3506-R1] depends on DataService + InventoryService +
ContainerConfig (display-only) + ItemConfig + RewardPoolConfig, writes AnalyzeSlots
(including the server-only, non-replicated ResultItemId) and Collection[ItemId], grants
Inventory.Items (production schema only), and sends AnalyzeResult (production metadata +
IsNew). Its reward identity is rolled ONCE and LOCKED into the persisted slot at Submit -
never memory-only, never rerolled by Claim/retry/reconnect/restart.

InventoryService owns Backpack.Containers and Inventory.Items. Actual Item insertion
accepts the unchanged prototype schema or the approved ItemUID-based production schema.
DepositService/WithdrawService consume only its Container surface.

DataService owns load/migrate/sanitize/save/replicate. It validates production ItemId through
ItemConfig and sends only its explicit public allowlist projection through DataUpdated.

EconomyService [UPDATED UO-3506-R1, UO-3506-R2, UO-3506-R3] depends on DataService +
InventoryService + item.Value + ActiveBuffs and writes Coins plus Collection[ItemId or
ItemType] (schema-aware key) TotalSold. A production item is now explicitly REJECTED before
it can reach the sell path at all (`isProductionItem` gate, UO-3506-R2) - it can never be
deleted for 0 Coins. [UO-3506-R3] Single Sell now reads the item via a read-only
`InventoryService.GetItem` peek before any mutation, so an ineligible item is rejected
without ever being removed. Value derivation for production Items remains a
full-Economy-migration gap (Hidden Coupling #3), but its silent-loss consequence is now
prevented rather than merely documented.
DataService [ADDED UO-3506-R2, SIMPLIFIED UO-3506-R4] also now depends on GameEnums +
RewardPoolConfig for the one-time legacy-Analyze-job ResultItemId reconciliation (section 6)
- a load-time-only concern, not a new runtime producer/consumer relationship. An unresolvable
legacy slot is retained and warned (quarantined), never refunded, deleted, or rerolled - the
previous Backpack/LabStorage refund attempt was unreachable dead code and has been removed.

DisplayService [UPDATED UO-3506-R1] depends on DataService + InventoryService + GemConfig and
writes Shelves[].Item plus ActiveBuffs. Now uses a schema-aware `setItemLocation` so placing/
removing a production item writes its `Location` field instead of corrupting it with a
stray `CurrentLocation` field (previously a destructive-mishandling bug - see Hidden Coupling
#2/section 9).

Collection identity is production-authoritative [UPDATED UO-3507/UO-3508]:
CollectionService owns production discovery writes and summaries keyed by valid ItemConfig
ItemId. The shared saved map may still retain sanitized legacy ItemType entries for prototype
compatibility, but production APIs and CollectionUI reject/ignore those keys through
ItemConfig.Get. CollectionUI now derives catalog placement and metadata from ItemConfig and
renders the production Collection[ItemId] projection. CollectionCatalog/GemConfig no longer
participate in the Collection UI path.

Client request flows send identifiers/intents only. Item-consuming clients read the
DataUpdated snapshot and GemConfig (except ClientBootstrap's reveal handler, which is fully
production-metadata-driven as of UO-3506-R1); no separate metadata-fetch remote exists.

================================================================================
MIGRATION READINESS SUMMARY
================================================================================

Production Ready / MIGRATED:
- ItemConfig (isolated, validated production foundation; now a live gameplay dependency via
  AnalyzeService, UO-3506).
- RewardPoolConfig (isolated, validated production foundation; UO-3505/UO-3505-R1; now a live
  gameplay dependency via AnalyzeService, UO-3506).
- AnalyzeService [UPDATED UO-3506/UO-3506-R1]: reward selection, production Item Instance
  creation, locked ResultItemId persisted at Submit (never rerolled), production Collection
  writes, and a production-metadata reveal payload are all COMPLETE. ContainerConfig reward
  ownership -> RewardPoolConfig is COMPLETE.
- InventoryService's dual-schema validation and schema-aware instance-key foundation.
- DataService's dual-schema Item validation/sanitization, newer-version refusal, and
  fail-closed public replication projection.
- Bootstrap's identifier/intent routing shape (payload contracts still require coordinated
  compatibility during migration).
- ClientBootstrap's Analyze reveal handler (UO-3506-R1): production metadata only, no
  GemConfig, no fake Quality/Value.

Minimal Compatibility Fixes Applied (UO-3506-R1) - NOT full migrations:
- ShopUI/ShelfUI: schema-aware instance key (ItemUID for production) and Location-aware
  placeable gating (ShelfUI), so a production item can actually be placed instead of the
  remote silently no-op'ing.
- EconomyService: schema-aware Location check and schema-aware Collection key (ItemId for
  production).
- DisplayService: schema-aware `setItemLocation` fixes a destructive bug where placing a
  production item on the Shelf permanently corrupted its schema and stranded it there
  (Hidden Coupling #2).

Sell Safety + Legacy Recovery Applied (UO-3506-R2) - NOT full migrations:
- EconomyService: a production Item is now explicitly NOT SELLABLE (`isProductionItem` gate
  in both single-Sell and Sell-All, checked before Location/Favorite) - it can never be
  deleted for 0 Coins. Superseded UO-3506-R1's "sells for 0 Coins" compatibility behavior.
- ShopUI: the Sell button is now clearly disabled ("Not sellable yet") for a production Item,
  matching the existing disabled-row pattern used for favorited/Shelf-displayed items.
- DataService: one-time, idempotent `reconcileAnalyzeSlots` recovers legacy Analyze jobs
  saved before `ResultItemId` existed - exactly one roll persisted per valid legacy slot,
  never rerolled again.

Sell Safety Hardened (UO-3506-R3) - NOT a full migration:
- EconomyService: single Sell no longer removes-then-restores an ineligible item. It now
  peeks the item read-only via `InventoryService.GetItem` FIRST and evaluates every
  eligibility check against that peek; `InventoryService.RemoveItem` is only ever called
  after an item is confirmed sellable, so an ineligible item (most importantly a production
  Item) is never touched at all.

Legacy Recovery Simplified (UO-3506-R4) - NOT a full migration, and recovery is NOT complete:
- DataService: the Backpack/LabStorage Container-refund attempt introduced in UO-3506-R2 and
  gated in UO-3506-R3 has been REMOVED as unreachable dead code - a slot only reached that
  branch by first failing to resolve its own Theme/Rarity against the Analyze mapping, and the
  refund's re-validation used that identical mapping on that identical data, so it could never
  actually succeed.
- DataService: a resolvable legacy slot (Theme/Rarity still maps to a valid Cave/ContainerTier
  with an eligible RewardPoolConfig pool) is still recovered fully - exactly one persisted
  `ResultItemId`, idempotent across repeated save/load.
- DataService: an unresolvable legacy slot is QUARANTINED, not recovered - retained exactly
  as-is (never deleted, never rerolled, never given an invented Cave/Rarity/ItemId/Container)
  with a clear server warning logged, and it CONTINUES OCCUPYING AN ANALYZE SLOT indefinitely.
  No automatic Container refund or other compensation exists. Reclaiming this slot requires a
  future, explicitly approved disposition mechanism that does not exist yet - a known, accepted
  operational limitation, not a bug.

Requires Migration:
- Remaining GemConfig consumers: DisplayService (Shelf Buff resolution), plus the prototype
  branches in InventoryUI, ShopUI, and ShelfUI.
- Remaining DefaultData/SaveData prototype compatibility and Shelf shapes.
- EconomyService and centralized Rarity-based sell-value configuration (Value derivation) -
  required before production Items can become sellable at all (currently blocked entirely,
  UO-3506-R2, not merely "sells for 0").
- DisplayService and production Shelf Buff resolution (formula-based resolver).
- ShopUI and ShelfUI full production-metadata presentation/payload.

Collection Migration Completed:
- UO-3507 centralizes production discovery and summary calculation in CollectionService.
- UO-3508 makes CollectionUI ItemConfig-driven, renders production Collection[ItemId], and
  removes CollectionCatalog/GemConfig from its runtime dependency path.

Blocked:
- None established by current approved design. Historical DQ-3 is not open. The existing
  Quality/Value dependencies require a coordinated implementation sequence, not a new
  product decision.

Unknown:
- None after direct source inspection.

Not part of Actual Item identity migration:
- AnalyzeMachineUI, BackpackUI, LabStorageUI (container/theme presentation).
- MiningService (container generation; no Actual Item identity).

================================================================================
HIDDEN COUPLING / MIGRATION RISK
================================================================================

1. [RESOLVED UO-3507/UO-3508] Collection identity used to be split between legacy
   ItemType save keys and the hand-authored CollectionCatalog UI grid. CollectionService now
   validates and writes production discoveries by ItemConfig ItemId, and CollectionUI now
   groups ItemConfig.GetAll() directly and checks the replicated Collection[ItemId] entries.
   Legacy keys may coexist only as sanitized prototype compatibility data and are ignored by
   production Collection APIs/UI. CollectionCatalog has no runtime consumer. Kept here as a
   historical record of the coupling that was removed.

2. Shelf Buff resolution is a hidden Analyze<->Shelf coupling through `ItemType`.
   DisplayService's `recalculateBuffs` reads `GemConfig.Gems[item.ItemType].BuffType`
   directly. If AnalyzeService is migrated to produce production ItemIds before
   DisplayService gains an equivalent production-side buff resolver, every Shelf Buff
   silently stops working (falls through to "no buff", not an error) the moment a
   production-identity item is placed on a Shelf - a correctness regression with no
   crash to signal it.

3. [MITIGATED UO-3506-R2] Economy payout depends on a field the production schema doesn't
   have. `EconomyService.getCoinsFor` reads `item.Value`, a per-instance prototype field.
   The approved production Item design (25_ITEM_CATALOG_DESIGN) has no Value field at all -
   sell value is meant to derive from Rarity through a centralized Economy config that does
   not yet exist. This USED TO mean every production sale would silently credit 0 Coins
   (value loss with no error). UO-3506-R2 closes that specific hole: `isProductionItem` now
   rejects a production Item before `creditSale`/`getCoinsFor` are ever reached, in both
   single-Sell and Sell-All - a production Item cannot be sold, and therefore cannot be
   deleted for 0 Coins, until the centralized Economy config (DQ-IC-01) exists. `getCoinsFor`
   itself is UNCHANGED (still reads `item.Value or 0`) and remains dead code for production
   Items specifically; the underlying "no centralized sell-value config" gap this coupling
   describes is NOT resolved, only its silent-value-loss CONSEQUENCE for production Items is
   prevented.

4. [RESOLVED UO-3506] RewardPool membership was inlined in ContainerConfig, not abstracted.
   `AnalyzeService.rollGem` used to read `ContainerConfig.Themes[theme].RewardPools[rarity]`
   directly. AnalyzeService now calls `RewardPoolConfig.HasPool`/`SelectRandomItemId`
   exclusively (section 4); `rollGem` no longer exists, and ContainerConfig.RewardPools is
   unreachable dead data (section 3). Kept here as a historical record of the coupling that
   was fixed, not a remaining risk.

5. Stale in-code documentation. CollectionCatalog's own module header still states the
   production ItemConfig "has NOT been built yet" and is "still listed as an open PHASE 0
   / roadmap task" - both now FALSE (ItemConfig was created in UO-3501, three tickets ago).
   A future implementer reading source comments instead of the Design Bible could be
   misled into re-creating ItemConfig or misjudging migration readiness. Not corrected
   here (CollectionCatalog is out of scope to modify per this ticket's constraints);
   flagged for correction whenever CollectionCatalog is next touched.

6. Dead code adjacent to the catalog. `ContainerConfig.RollRarity()` is no longer called
   by Mining (which uses `BalanceConfig.Mining.BaseRarityRates` since UO-1005) but remains
   present and could be mistaken for live rarity-roll logic during a RewardPoolConfig
   migration. Not a functional risk today; a documentation/cleanup risk during migration.

7. [RESOLVED UO-3506-R1] Analyze result identity was not durable: `pendingItems` existed
   only in server memory, and `recoverPendingReward` made a new random roll after restart.
   AnalyzeService now rolls and LOCKS `ResultItemId` directly onto the persisted slot at
   Submit; `pendingItems`/`recoverPendingReward` no longer exist. resolveSlot, HandleClaim,
   and ResumePlayer all read the same persisted value back and never reroll it, across
   Claim, retry, reconnect, or restart. Kept here as a historical record of the bug that was
   fixed, not a remaining risk.

8. [RESOLVED UO-3506-R1] Hidden-result persistence was flagged as coupled to replication;
   UO-3504's fail-closed public allowlist (DataService.PUBLIC_PROFILE_SCHEMA.AnalyzeSlots)
   already excluded ResultItemId before this field was ever populated. Now that
   AnalyzeService actually persists `ResultItemId` on every slot (UO-3506-R1), it is
   confirmed still absent from the allowlist and therefore never reaches the client via
   DataUpdated before Claim, exactly as Design/04 requires.

9. DataService still contains the live v3-to-v4 legacy writer that creates prototype
   ItemType/Quality/Value records. UO-3504 adds dual-schema sanitization and refuses profiles
   newer than this server supports; the later clean schema migration must still coordinate
   retirement of the legacy writer.

10. Quality removal crosses multiple systems. DataService/InventoryService now accept the
    approved production shape without Quality/Value, but AnalyzeService compares item.Quality
    when updating BestQuality;
    EconomyService pays from Value; InventoryUI/ShopUI/ShelfUI/ClientBootstrap display the
    fields. Switching the producer first can reject grants, error during Collection update,
    pay zero Coins, or render misleading Q0/Value0 output. CollectionUI itself does not use
    Quality or Value.

================================================================================
RECOMMENDED MIGRATION ORDER (WITH JUSTIFICATION)
================================================================================

This is a recommendation only; it does not create, define, or assign a number to the
future runtime migration ticket, whose placeholder remains TBD. It preserves the canonical
catalog -> Analyze -> Collection -> Shelf
-> Economy -> UI direction while applying 13_IMPLEMENTATION_PLAN/17_SAVE_SYSTEM's
projection-first and expand-migrate-contract requirements. No step may leave a supported
runtime unable to read the identity shape currently being produced.

1. BOUNDARIES FIRST: define the clean versioned Item/Analyze/Collection/Shelf schemas,
   shared validators/sanitizers, locked persisted ResultItemId contract, and redacted
   public projection/DTO. The reset policy removes prototype-profile conversion, not this
   clean-schema work.

2. PRODUCTION CONFIG PREREQUISITES: RewardPoolConfig under DQ-IC-05 now exists (UO-3505,
   UO-3505-R1; see section 1B). Still needed: the centralized Rarity-based Economy
   sell-value configuration under DQ-IC-01, and the production Shelf Buff resolver
   interface required by approved design. These foundations do not switch live producers
   by themselves.

3. EXPAND COMPATIBILITY: make Inventory validation, DataService load/save/projection,
   Collection writes/reads, Economy, Display, and affected UI/payload readers capable of
   safely handling the production identity shape before Analyze begins emitting it.
   Preserve supported legacy reads during the rollback window; avoid an unsupported
   producer-first hard cutover.

4. [COMPLETE - UO-3506/UO-3506-R1] ANALYZE VERTICAL SLICE: switch reward selection and
   Actual Item grants to authoritative ItemIds, persist the locked ResultItemId, redact it
   before Claim, and coordinate the Claim-time Collection write with the production schema.
   Validate full-inventory, restart/rejoin, save/load, and reveal behavior as one slice.

5. [COMPLETE - UO-3507/UO-3508] COLLECTION MIGRATION: CollectionService now owns
   production ItemId discovery/summary behavior, and CollectionUI derives catalog presentation
   directly from ItemConfig. The hand-authored CollectionCatalog path has no runtime consumer.

6. SHELF MIGRATION: switch Shelf metadata/buff resolution to the production resolver
   without a period where production-ID items silently provide no buff.

7. ECONOMY MIGRATION: switch payout to the centralized Rarity-based configuration without
   a period where production items sell for zero.

8. UI MIGRATION: ItemConfig-backed Inventory, Collection, and Analyze reveal presentation
   are complete. Shop/Shelf production presentation remains pending; final names/visuals remain
   dependent on UO-3600.

9. CONTRACT: CollectionUI's legacy CollectionCatalog/GemConfig reads and Analyze's
   ContainerConfig RewardPool read are already removed. Remove the remaining legacy GemConfig
   reads only after their supported prototype paths and rollback requirements are retired.

Pre-existing Economy/Display proximity hardening remains separately scoped and is not a
catalog-migration prerequisite.

================================================================================
VALIDATION SUMMARY
================================================================================

- Every system named in ACTIVE_TICKET is covered, including Bootstrap remotes,
  DataUpdated replication, persisted fields, and client request/presentation flows.
- Project-wide references to ItemConfig, GemConfig, ContainerConfig, CollectionCatalog,
  ItemType, ItemId, Quality, Value, ResultItemId, pendingItems, DataUpdated, and Item
  remotes were traced to their readers/writers.
- ItemConfig has SIX current runtime consumers [UPDATED THROUGH UO-3508]:
    ItemConfig
    ├─ DataService (production ItemId validation)
    ├─ RewardPoolConfig (production reward-pool construction)
    ├─ AnalyzeService (Submit/Claim validation)
    ├─ CollectionService (production discovery/summary)
    ├─ InventoryUI (production Item presentation)
    └─ CollectionUI (production catalog/discovery/progress presentation)
  AnalyzeService is now a live gameplay consumer of both ItemConfig and RewardPoolConfig, and
  every new Actual Item production ItemId reaches a player's Inventory through it. Prototype
  Items already sitting in a player's save from before this migration remain legacy GemConfig
  keys until a future migration/reset touches them (17_SAVE_SYSTEM development reset policy).
- 17_SAVE_SYSTEM marks DQ-2 through DQ-8 historical/removed from the critical path; no
  open DQ-3 blocker is asserted here.
- GemConfig's current header and data use the locked six production rarities.
- The migration recommendation's step 4 (ANALYZE VERTICAL SLICE) and step 5
  (COLLECTION MIGRATION) are COMPLETE. CollectionService is the production discovery/summary
  authority, CollectionUI consumes ItemConfig and replicated Collection[ItemId], and
  CollectionCatalog has no runtime consumer.
- [UO-3506-R1] Verified the ResultItemId persistence/locking, Claim-safety (grant-then-clear,
  retain-on-failure), Collection-write, reveal-payload, and minimal Shop/Shelf/Economy/
  Display compatibility fixes directly against the current source (see sections 4, 8, 9, 11
  and Hidden Coupling #4/#7/#8 above).
- [UO-3506-R2] Verified directly against the current source: production Items are rejected
  by `isProductionItem` before `creditSale` in BOTH EconomyService sell paths - selling one
  for 0 Coins is confirmed NOT ACCEPTED BEHAVIOR (see section 8, Hidden Coupling #3);
  prototype selling is unaffected. ShopUI's Sell button is confirmed disabled with a clear
  "Not sellable yet" reason for production Items (section 11).
- [UO-3506-R3] Verified directly against the current source and in Studio (cloned-module
  tests): EconomyService's single Sell peeks via `InventoryService.GetItem` before any
  mutation and never calls `RemoveItem` for an ineligible item - the prior remove-then-restore
  pattern no longer exists (section 8).
- [UO-3506-R4] Verified directly against the current source and in Studio (cloned-module
  tests): the unreachable Backpack/LabStorage Container-refund branch has been removed from
  `reconcileAnalyzeSlots` entirely - this function no longer reads or writes Backpack or
  LabStorage at all, and no Container is ever created by it. A resolvable legacy slot still
  rolls exactly once and remains idempotent across a live Load -> Save -> Release -> Load
  round-trip, with the same `ResultItemId` preserved byte-for-byte. An unresolvable slot
  (invalid Theme/Rarity) is confirmed unchanged - SlotId, Theme, Rarity, and the absence of
  `ResultItemId` all preserved across a subsequent load - with a clear server warning logged
  and no Backpack/LabStorage container ever created. A malformed Backpack (non-table) was
  confirmed not to crash `Load`, now trivially true since Backpack is never touched by this
  function. EconomyService and ShopUI were confirmed unchanged from the approved UO-3506-R3
  sell-safety implementation (section 8, section 11).
- Zero runtime files were modified to produce or correct the ORIGINAL UO-3503 audit; this
  UO-3506/UO-3506-R1/UO-3506-R2/UO-3506-R3/UO-3506-R4 update DOES describe runtime changes
  made by those tickets (documented above), matching this document's stated purpose of
  tracking the current implementation.
