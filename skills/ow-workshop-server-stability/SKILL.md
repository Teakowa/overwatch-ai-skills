---
name: ow-workshop-server-stability
description: Audit and remediate Overwatch Workshop server-load risks using a repeatable workflow for loops, conditions, startup load, event pressure, and anti-crash fallback rules. Use when diagnosing crashes, server-load spikes, or unstable behavior in production lobbies.
license: AGPL-3.0-only
compatibility: Designed for Agent Skills-compatible coding agents, including Codex and Claude Code. SKILL.md is canonical; agents/openai.yaml is optional UI metadata. Requires local filesystem access only.
metadata:
  author: Teakowa
  version: "1.0.0"
  canonical_spec: "https://agentskills.io/specification"
  target_products: "Codex, Claude Code"
---

# OW Workshop Server Stability

## Overview
Use this skill to debug and harden Workshop logic that is causing server-load spikes or crashes.

This skill is operational, not tutorial-first. It gives a repeatable audit loop:

1. classify the symptom
2. isolate expensive paths
3. apply the minimum safe fix pattern
4. re-check behavior under multi-player load

Primary source references:

- [Improve your code's server stability](https://workshop.codes/wiki/articles/improve-your-codes-server-stability)
- [How to use loops](https://workshop.codes/wiki/articles/how-to-use-loops)

## Quick Start
1. Identify incident class:
: startup crash, periodic spikes, loop-heavy overload, damage-event overload, or sustained high baseline.
2. Locate rules with high fanout:
: `Ongoing - Each Player`, large arrays in conditions, frequent `Player Dealt/Took Damage`, waitless loops.
3. Apply one remediation pattern at a time.
4. Re-test in realistic load (multiple players/bots), not only low-load local scenarios.
5. Keep anti-crash fallback only as a safety net, not as primary optimization.

## Stability Audit Workflow
1. Classify symptom.
: Startup crash usually points to too many startup conditions or synchronized heavy initialization.
: Periodic spikes usually point to batched expensive actions, replay-related peaks, or synchronized per-player work.
: Per-event overload usually points to high-frequency event rules with costly action chains.
2. Isolate expensive paths.
: Start from rules with expensive first conditions (`Distance`, `Is True For Any` on large arrays).
: Check loop paths for missing wait throttles.
: Check event filters (`All` vs hero/slot filters).
3. Apply targeted remediation.
: Prefer low-risk ordering/filtering/throttling changes before structural rewrites.
4. Re-check and compare.
: Confirm symptom reduction and acceptable gameplay precision.
: Keep a note of precision tradeoff if interval-based checks are introduced.

## Remediation Patterns

### 1) Wait In Repeating Paths
Use for rule loops and `While` blocks that can run indefinitely.

```opy
// Rule loop throttle
Wait(0.200, Ignore Condition);
Loop If Condition Is True;

// While loop throttle
While(condition);
    // actions
    Wait(0.016, Ignore Condition);
End;
```

When not to use:

- If the behavior requires per-tick precision and any delay breaks gameplay semantics.

### 2) Sparse Evaluation (`Wait + If + Loop`)
Move very expensive conditions from `conditions` into action flow when exact tick-level precision is not required.

```opy
rule("Expensive check (sampled)")
{
    event
    {
        Ongoing - Each Player;
        All;
        All;
    }

    actions
    {
        Wait(0.200, Ignore Condition);
        If(Is True For Any(Global.zones, Distance Between(Event Player, Current Array Element) < 5));
            Kill(Event Player, Null);
        End;
        Loop;
    }
}
```

When not to use:

- If sampling delay causes unacceptable misses (for example very fast collision windows).

### 3) De-sync Expensive Startup Work
Offset heavy per-player initialization so it does not align on one tick.

```opy
Wait(Slot Of(Event Player) * 0.016, Ignore Condition);
```

When not to use:

- If synchronized behavior is a core gameplay requirement and cannot be delayed per player.

### 4) Anti-Crash Fallback Rule
Use only as emergency protection for transient spikes.

```opy
rule("Anti-Crash")
{
    event
    {
        Ongoing - Global;
    }

    conditions
    {
        Server Load > 230;
    }

    actions
    {
        Wait(1, Abort When False);
        Set Slow Motion(10);
        Wait Until(Server Load < 200, 99999);
        Set Slow Motion(100);
    }
}
```

When not to use:

- As a substitute for fixing root-cause performance problems.
- In experiences where strong slow-motion degradation is unacceptable.

## Failure Modes and Tradeoffs
- Waitless loops:
: Maximum responsiveness, highest crash risk.
- Sparse evaluation:
: Lower load, reduced precision due to sampling interval.
- Aggressive startup deferral:
: Lower startup spike, potentially delayed UI/effect readiness.
- Damage-event throttling:
: Lower event pressure, can skip intended repeated reactions.
- Anti-crash slowdown:
: Avoids hard crashes, but degrades player experience.

Practical rule of thumb:

- fix ordering/filtering first,
- then add throttling,
- then use anti-crash fallback as last-line protection.

## Review Checklist
- [ ] Every self-repeating path includes a `Wait`.
- [ ] Expensive conditions are not first-gate checks unless unavoidable.
- [ ] Hero/slot-specific rules use event player filters where possible.
- [ ] Startup heavy rules are deferred or de-synced.
- [ ] Large-array condition checks are sampled or otherwise bounded.
- [ ] High-frequency damage-event rules are throttled with known side effects.
- [ ] Anti-crash logic exists only as fallback, not primary optimization.
- [ ] Changes verified under multi-player load profile.

## Attribution
This skill is a structured operational rewrite based on:

- Local source: `docs/improve-server-stability.md` in Overwatch-AI-PVE.
- Upstream tutorial: [Improve your code's server stability](https://workshop.codes/wiki/articles/improve-your-codes-server-stability).
- Related loop guidance: [How to use loops](https://workshop.codes/wiki/articles/how-to-use-loops).
