# 02 — AI Handbook

## Authority hierarchy

Use this order when sources conflict:

1. Explicitly approved current design decisions
2. `01_PROJECT_STATE.md`
3. `ACTIVE_TICKET.md`
4. Authoritative design documents
5. Technical architecture documents
6. Approved decision records
7. Current runtime implementation
8. Prototype documents
9. Chat history
10. AI memory or assumptions

Runtime code is evidence of current behavior, not automatic proof of intended final design.

## Mandatory workflow

### Before implementation

1. Read `00_START_HERE.md`.
2. Read `01_PROJECT_STATE.md`.
3. Read `ACTIVE_TICKET.md`.
4. Read every design and runtime source named by the ticket.
5. Identify contradictions before changing code.
6. Update `ReplicatedStorage/Design/ACTIVE_TICKET` before implementation when working inside the Roblox project.
7. Do not modify files outside ticket scope.

### During implementation

- Prefer the smallest safe change.
- Preserve established architecture unless the ticket explicitly authorizes a refactor.
- Do not duplicate configuration.
- Do not create a new source of truth inside UI, services, or gameplay code.
- Do not invent missing mechanics, content, values, names, mappings, or migration rules.
- Mark unresolved matters as `DESIGN QUESTION`.
- Keep prototype behavior explicitly labeled as prototype behavior.
- Maintain backward compatibility unless the ticket explicitly permits breaking changes.
- Use server authority for trusted gameplay and persistence decisions.
- Keep clients focused on presentation and input.

### After implementation

Return:

- ACTIVE_TICKET updated: Yes/No
- Files created
- Files modified
- Behavior implemented
- Tests performed
- Known limitations
- DESIGN QUESTIONS
- Runtime code changed: Yes/No
- Config modules changed: Yes/No
- Save schema changed: Yes/No
- Ready for independent review: Yes/No

## Ticket writing standard

Unknown Ore implementation tickets should be concise and use:

1. ACTIVE_TICKET instruction
2. Goal
3. Existing Systems
4. Requirements
5. Constraints
6. Deliverables
7. Acceptance Criteria
8. Implementation Report Checklist

Avoid unnecessarily long prompts unless architectural risk requires detail.

## Review standard

Architecture-sensitive work requires an independent review session.

The reviewer must:

- not modify files
- verify ticket compliance
- verify cross-document consistency
- inspect relevant current runtime behavior
- distinguish critical findings from warnings
- recommend exactly one next action

## Source-of-truth rules

- `ItemConfig` owns collectible identity and shared item metadata once approved and implemented.
- `ContainerConfig` owns container/reward behavior, not item identity.
- Analyze chooses or reveals an ItemId; it does not define the item.
- Inventory stores item instances and references ItemIds.
- Collection stores discovery state and presentation ordering; it does not define the catalog.
- Shelf consumes catalog metadata; it does not define items.
- Economy consumes catalog or economy metadata; it does not define items.
- UI renders authoritative data; it does not duplicate it.

## Prohibited behavior

- Inventing final names for unresolved item slots
- Silently declaring candidate IDs final
- Assigning legacy items to production slots without approval
- Treating prototype reward pools as final production design
- Continuing Collection implementation before required catalog migration work
- Replacing architecture because a different pattern seems cleaner
- Hiding contradictions instead of reporting them
