# OverPy and OSTW Bridge Notes

Use this file when translating ideas between Workshop pseudo-logic, OverPy, and OSTW.

## Scope
- Provide practical mapping heuristics.
- Provide side-by-side examples for common patterns.
- Avoid claiming compiler-specific behavior unless documented in current project/tooling docs.

## Mapping Model

### Concept Layer
- Workshop Rule -> OverPy rule block -> OSTW rule block
- Workshop Event -> Event declaration section
- Workshop Condition -> Condition clause/expression list
- Workshop Actions -> Ordered action statements
- Workshop Values -> Expressions usable in conditions/actions

### Execution Layer
- Workshop executes actions in order.
- Repetition constructs (`Loop`, `While`, `For`) require explicit throttling where needed.
- Event scope (global vs each-player) determines available contextual variables.

## Translation Workflow
1. Normalize source behavior into event/condition/actions form.
2. Preserve event scope and ownership of state (global vs player variables).
3. Preserve timing semantics (`Wait`, loop interval, timeout).
4. Translate syntax with minimal structural drift.
5. Re-check behavioral equivalence under load-related constraints.

## Side-by-Side Pattern Templates

### Template A: Throttled Polling Rule
Workshop-style idea:
```text
Every player: while condition true, do action every 0.2s.
```

OverPy-style skeleton:
```py
rule("Throttled Poll"):
    event
    {
        Ongoing - Each Player;
        All;
        All;
    }

    conditions
    {
        <condition> == True;
    }

    actions
    {
        <action>;
        Wait(0.200, Ignore Condition);
        Loop If Condition Is True;
    }
```

OSTW-style skeleton:
```text
rule("Throttled Poll") {
  event { Ongoing - Each Player; All; All; }
  conditions { <condition> == True; }
  actions {
    <action>;
    Wait(0.200, Ignore Condition);
    Loop If Condition Is True;
  }
}
```

### Template B: Bounded Iteration
Workshop-style idea:
```text
Iterate targets and apply action.
```

Translation checklist:
- Keep explicit index variable.
- Keep explicit upper bound.
- Add batching waits for large arrays.

### Template C: Event-Specific Reaction
Workshop-style idea:
```text
On damage event, react with lightweight filtered actions.
```

Translation checklist:
- Keep event-specific actor variables.
- Add early exits/filters.
- Avoid expensive scans in high-frequency event handlers.

## Common Translation Errors
- Dropping event scope during translation.
- Changing loop termination semantics.
- Removing `Wait` from a repeating path.
- Replacing bounded iteration with open-ended loops.
- Moving expensive checks to per-tick Conditions without throttling.

## Source Anchors
- workshop.codes root: https://workshop.codes
- workshop.codes wiki index (md mirror): https://md.owbastion.codes/wiki/articles.md
- md.owbastion.codes root: https://md.owbastion.codes
