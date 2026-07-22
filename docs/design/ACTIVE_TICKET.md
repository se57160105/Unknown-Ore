UO-3508 — Collection UI Production Integration

Goal
Connect Collection UI to the production CollectionService and ItemConfig.

Existing Systems

* CollectionService
* ItemConfig
* Collection UI (prototype)
* Collection Summary API

Requirements

* Populate Collection UI entirely from ItemConfig.
* Use CollectionService to determine discovered/undiscovered state.
* Display production metadata (DisplayName, Cave, Rarity).
* Undiscovered items must use placeholder visuals/text.
* Display:
   * Overall completion
   * Per-cave progress
   * Per-rarity progress
* Refresh automatically after a new discovery.
* UI must not cache catalog data permanently.

Constraints

* No reward system.
* No Shelf integration.
* No Economy changes.
* No Collection data mutation from UI.
* No hardcoded catalog values.

Deliverables

* Production Collection UI.
* Runtime UI refresh.
* Placeholder support.
* Summary display integration.
* Updated ACTIVE_TICKET.

Acceptance Criteria

* Every production item appears in Collection.
* Newly discovered items reveal immediately.
* Undiscovered items remain hidden.
* Progress matches CollectionService.
* UI works after reconnect/rejoin.
* No prototype catalog dependency.

Implementation Report Checklist

* Files inspected/modified
* UI bindings updated
* Runtime refresh verified
* Manual tests
* Known limitations
