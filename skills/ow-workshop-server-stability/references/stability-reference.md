# Server Stability Reference

## Symptom -> Cause -> Fix

| Symptom | Likely cause | Preferred fix |
| --- | --- | --- |
| Crash near match start | Too many startup conditions or synchronized heavy init | Defer checks by spawn/time/living-player gate and de-sync startup waits |
| Periodic load spikes | Heavy actions aligned on same ticks | Split chains and insert short waits; per-player offset waits |
| Continuous high baseline load | Expensive condition-first rules on many players | Reorder conditions and move expensive checks into sparse evaluation loops |
| Crash during ability spam | Waitless loops or high-frequency event actions | Add waits, batch expensive actions, bound per-tick workload |
| Instability in damage rules | Too many `Player Dealt/Took Damage` triggers + heavy actions | Add cautious throttling and choose event side (`Dealt` vs `Took`) by desired fanout |
| Rare spike crashes | Temporary overload beyond safe threshold | Keep anti-crash fallback with clear activation and release thresholds |

## Costly Constructs

| Construct | Risk | Safer pattern |
| --- | --- | --- |
| Large arrays in rule conditions | Any array update can trigger broad re-evaluation | Cache/split data and sample with `Wait + If + Loop` |
| Expensive first conditions (`Distance`, `Is True For Any`) | High evaluation cost on every tick/player | Put cheap/high-selectivity checks first |
| Many long action chains in one frame | Tick-time burst and spike risk | Insert short waits and batch in chunks |
| Waitless repeating loops | Infinite same-frame pressure | Add `Wait` in all repeating paths |
| Uniform heavy startup routines | Same-tick peak at match start | De-sync using slot-based or shard-based offsets |

## Fast Triage Checklist

1. Confirm whether the issue is startup-only, periodic, or input/event burst related.
2. Inspect repeating paths first (`Loop`, `While`, long `For` chains).
3. Inspect first conditions of each hot rule for expensive operations.
4. Add one remediation at a time and retest with realistic concurrency.
5. Document precision tradeoffs whenever evaluation intervals are increased.
6. Keep anti-crash fallback enabled only after root-cause fixes are attempted.
