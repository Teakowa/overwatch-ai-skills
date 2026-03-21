# Workshop Wiki Index Map

Use this file as the primary navigation map for workshop.codes and md.owbastion.codes lookups.

## Core Entrypoints
- workshop.codes wiki root: https://workshop.codes/wiki
- workshop.codes articles index: https://workshop.codes/wiki/articles
- workshop.codes docs root (legacy-style nav): https://workshop.codes
- md.owbastion.codes root: https://md.owbastion.codes

## Domain Routing

### 1) Actions and Control Flow
Use for: action semantics, control flow correctness, and execution order.

High-priority pages:
- Loop: https://workshop.codes/wiki/articles/loop
- Loop If: https://workshop.codes/wiki/articles/loop+if
- Loop If Condition Is True: https://workshop.codes/wiki/articles/loop+if+condition+is+true
- Loop If Condition Is False: https://workshop.codes/wiki/articles/loop+if+condition+is+false
- While: https://workshop.codes/wiki/articles/while
- End: https://workshop.codes/wiki/articles/end
- Wait: https://workshop.codes/wiki/articles/wait
- If / Else / End If group: https://workshop.codes/wiki/articles

### 2) Events and Rule Context
Use for: trigger selection, per-player/global behavior, event pressure.

Lookup strategy:
- Start from article index and filter by event name.
- Prioritize exact event context in answers (global vs each-player, team filters, hero filters).

Common event families:
- Ongoing events
- Damage/heal events
- Elimination/death events
- Button/input events

### 3) Values and Conditions
Use for: value type expectations, condition cost, and expression composition.

Lookup strategy:
- Confirm value return type and valid argument types.
- Validate whether value is expensive (distance scans, array predicates, LOS checks).

Common expensive-pattern indicators:
- Repeated distance checks in Conditions
- Large array predicates per tick
- Multi-entity scans under each-player ongoing events

### 4) Workshop Idioms
Use for: stable implementation patterns and repeated design motifs.

Idioms to apply:
- `Wait + Loop` throttled polling for expensive checks
- `For` loops for bounded iteration over arrays/ranges
- Rule split by concern (sensing, state update, effects)
- Slot-based startup desync (`Wait(Slot Of(...) * delta)`) to avoid synchronized spikes

### 5) Performance and Stability
Use for: server-load spikes, crash risk, and lobby-scale instability.

Priority references:
- Improve server stability: https://workshop.codes/wiki/articles/improve-your-codes-server-stability
- How to use loops: https://workshop.codes/wiki/articles/how-to-use-loops

## Suggested Lookup Sequence
1. Identify object type in user question: action, value, event, or architecture issue.
2. Resolve to domain section above.
3. Open the narrowest relevant article(s).
4. Verify context assumptions (event scope, loop frequency, target population).
5. Return answer with links and practical caveats.

## Citation Rules
- Always cite at least one concrete source URL when using factual behavior claims.
- If no exact page is available, cite index/root links and mark claims as inferred.
- Prefer specific article URLs over root URLs when both exist.
