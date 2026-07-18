# Source of Truth Map

| Domain | Authoritative source | Consumers | Must not own |
|---|---|---|---|
| Current project status | `01_PROJECT_STATE.md` | All AI and developers | Detailed system design |
| Current work | `ACTIVE_TICKET.md` | Implementer and reviewer | Permanent project history |
| Game rules | Relevant design documents | Runtime systems | Runtime implementation details |
| Collectible identity | `ItemConfig` after UO-3501 | Analyze, Inventory, Collection, Shelf, Economy, UI | Reward behavior or player state |
| Container behavior | `ContainerConfig` | Mining and Analyze | Item definitions |
| Item instance state | Inventory/Data schema | Services and UI | Global item metadata |
| Collection discovery | Collection save schema/service | Collection UI | Item definitions |
| Shelf placement | Shelf save schema/service | Shelf UI and buffs | Item definitions |
| Economy formulas | Economy config/design | EconomyService | Item identity |
| Work history | Changelog and completed tickets | All | Current active requirements |
| Unresolved decisions | Decision queue / project state | Ticket author | Silent assumptions |

## Conflict rule

When two sources disagree, do not choose silently. Report the conflict and use the authority hierarchy in `02_AI_HANDBOOK.md`.
