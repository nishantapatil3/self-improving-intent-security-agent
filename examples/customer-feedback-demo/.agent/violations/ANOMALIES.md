# Anomaly Detection Log

Track unusual behavioral patterns detected during execution.

---

## [ANO-20260327-001] pattern_deviation

**Detected**: 2026-03-27T17:01:46Z
**Severity**: medium
**Intent**: INT-20260327-001
**Status**: resolved

### Anomaly Details
The agent began considering a housekeeping action on source files even though the active task was limited to analysis and report generation.

### Evidence
- Metric that triggered alert: `source_mutation_candidate_count`
- Baseline value: `0`
- Actual value: `1`
- Deviation: `+1 candidate action`
- Timeline: stable read-only execution from 17:00:12Z to 17:01:45Z, deviation detected at 17:01:46Z

### Assessment
This was anomalous because all earlier actions were read-only and aligned with the intent. Introducing a source-file move indicated drift from task execution into workspace cleanup.

### Type
- [x] Goal Drift
- [ ] Capability Misuse
- [x] Side Effects
- [ ] Resource Exceeded
- [x] Pattern Deviation

### Response Taken
- [ ] Continued with monitoring
- [x] Applied constraints
- [x] Rolled back to checkpoint
- [ ] Halted execution
- [ ] Requested clarification

### Metadata
- Related Intent: INT-20260327-001
- Threshold: `0 source mutations allowed`
- False Positive: no
- See Also: VIO-20260327-001

---
