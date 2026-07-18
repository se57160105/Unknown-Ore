# 00 — Start Here

## Purpose

This file is the boot sequence for every AI or developer working on Unknown Ore.

## Required reading order

1. `01_PROJECT_STATE.md`
2. `02_AI_HANDBOOK.md`
3. `SOURCE_OF_TRUTH_MAP.md`
4. `../tickets/active/ACTIVE_TICKET.md`
5. Related design documents
6. Related architecture documents
7. Recent decisions and reviews, when relevant

## Before implementation

- Confirm the active ticket is current.
- Confirm the task does not conflict with locked decisions.
- Read the exact runtime files named by the ticket.
- Treat unresolved requirements as `DESIGN QUESTION`.
- Do not invent missing game mechanics, item names, values, mappings, or migration behavior.
- Do not allow a gameplay system to become the authority for shared content identity.

## After implementation

- Return an implementation report.
- Update project state only when the ticket explicitly requires it.
- Record important approved changes in the changelog.
- Run an independent review before moving to the next architecture-sensitive ticket.

## Context restoration command

When starting a new AI session, use:

> Read `docs/project-os/00_START_HERE.md` and follow its required reading order before proposing or modifying anything.
