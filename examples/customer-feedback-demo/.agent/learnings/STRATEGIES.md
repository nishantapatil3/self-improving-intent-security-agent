# Strategy Registry

---

## [STR-20260327-001] immutable_source_derived_index

**Created**: 2026-03-27T17:03:18Z
**Status**: adopted
**Source Learning**: LRN-20260327-001
**Domain**: file_processing
**Version**: 1.0.0

### Strategy
When source files are constrained as immutable:
- inspect and classify in read-only mode
- write duplicate, spam, or exception indexes to an approved output directory
- never move, rename, or archive source material without explicit user approval

### Expected Benefit
- lower violation rate for file-processing tasks
- clearer audit trails
- better preservation of user trust and data integrity

### Evidence
- Sample size: 1 illustrative run
- Violation prevented: yes
- Rollback-assisted recovery: yes
- Outcome after correction: successful completion

### Rollout Guidance
- Default for all tasks with "do not modify originals" constraints
- Reevaluate after 10 production runs
- Escalate to user if cleanup seems materially useful but not explicitly requested

---
