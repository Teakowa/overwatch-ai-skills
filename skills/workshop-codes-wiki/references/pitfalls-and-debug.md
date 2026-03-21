# Pitfalls and Debug Recipes

Use this file when diagnosing incorrect behavior, instability, or performance regressions.

## High-Risk Pitfalls

### 1) Waitless Repeating Paths
Symptoms:
- Server load rises rapidly.
- Lobbies stutter or crash.

Detection:
- Rule loops without `Wait`.
- `While` blocks without `Wait`.

Remediation:
- Add `Wait(...)` in every self-repeating path.
- Use interval values based on required precision.

### 2) Expensive Conditions at High Frequency
Symptoms:
- Sustained high baseline server load.
- Gradual instability with more players/entities.

Detection:
- Distance/array predicate checks in Conditions every tick.
- Broad target scans under `Ongoing - Each Player`.

Remediation:
- Move expensive checks into `Wait + If + Loop` action flow.
- Add early cheap filters before expensive checks.

### 3) Event Fanout Overload
Symptoms:
- Spikes on damage-heavy scenes.
- Delays in unrelated rules.

Detection:
- Heavy action chains inside high-frequency events.
- Missing filters for hero/team/state.

Remediation:
- Add narrow filters early.
- Split heavy work into deferred routines.
- Throttle non-critical reactions.

### 4) Startup Synchronization Spikes
Symptoms:
- Immediate lag/crash at round start.

Detection:
- Many players run heavy init logic on same tick.

Remediation:
- Desync startup using slot-based wait offsets.
- Stage initialization into lightweight phases.

## Debug Workflow
1. Classify incident: crash, baseline overload, periodic spike, or event burst.
2. Isolate top-risk rules: loops, each-player scans, damage events.
3. Apply smallest safe change first.
4. Re-test with multi-player/bot load.
5. Keep notes on precision-vs-performance tradeoffs.

## Practical Guardrails
- Never keep a repeating path unthrottled.
- Prefer bounded loops over open-ended loops when possible.
- Keep expensive logic out of always-on conditions unless mandatory.
- Scope rules tightly (team/hero/state) before expensive operations.
- Use fallback protections only as temporary safety net, not root fix.

## Reference Links
- Stability guidance: https://workshop.codes/wiki/articles/improve-your-codes-server-stability
- Loop guidance: https://workshop.codes/wiki/articles/how-to-use-loops
- Wiki index: https://workshop.codes/wiki/articles
