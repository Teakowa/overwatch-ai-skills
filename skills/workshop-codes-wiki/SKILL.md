---
name: workshop-codes-wiki
description: Provide high-coverage documentation support for Overwatch Workshop, OverPy, and OSTW development using workshop.codes wiki and md.owbastion.codes references. Use when answering syntax/semantics questions, mapping Workshop logic to OverPy or OSTW, clarifying actions/events/values/rules, troubleshooting server-load or stability issues, or producing source-backed implementation guidance.
---

# Workshop.Codes Wiki Support

## Overview
Use this skill to answer Workshop, OverPy, and OSTW developer questions with source-backed, implementation-ready guidance.

Prefer English output by default. Switch to Chinese only when the user explicitly requests Chinese.

## Workflow
1. Classify the request into one primary intent.
: `lookup`, `translation`, `debug`, `performance`, or `architecture`.
2. Select the minimal reference set.
: Start from `references/wiki-index.md`, then load only the domain files needed.
3. Produce an actionable answer.
: Give concrete steps or code patterns first, then short rationale.
4. Cite sources.
: Include workshop.codes and/or md.owbastion.codes links used for the answer.
5. List caveats.
: Mention precision/performance tradeoffs, unsupported assumptions, and version-sensitive points.

## Intent Routing
- `lookup` (action/event/value meaning, valid usage, edge behavior)
: Use `references/wiki-index.md` first.
- `translation` (Workshop <-> OverPy <-> OSTW)
: Use `references/overpy-ostw-bridge.md` first, then confirm with wiki entries.
- `debug` (unexpected behavior, wrong target, invalid condition ordering)
: Use `references/pitfalls-and-debug.md` first.
- `performance` (server-load spikes, crashes, event storms)
: Use `references/pitfalls-and-debug.md` and stability-related entries from `wiki-index.md`.
- `architecture` (rule decomposition, modularization, maintainability)
: Use all three reference files, then provide a staged refactor path.

## Output Contract
- Write in English unless the user asks for Chinese.
- Provide direct, implementation-oriented guidance before theory.
- Include at least one relevant source link when factual claims depend on docs.
- When translating between Workshop/OverPy/OSTW, provide side-by-side examples if feasible.
- If docs are ambiguous, say what is certain vs inferred.

## Terminology Mapping
Use these normalized terms when users mix vocabularies:

- Workshop Action -> OverPy action call / OSTW action statement
- Workshop Value -> OverPy expression / OSTW expression
- Rule Event -> Event block trigger
- Ongoing - Each Player -> per-player ongoing event context
- Waitless loop risk -> unthrottled repeat path risk

When terms conflict, mirror user wording once, then normalize to one vocabulary for the rest of the answer.

## Reference Usage Rules
- Treat `references/wiki-index.md` as the navigation map.
- Treat `references/overpy-ostw-bridge.md` as translation guidance, not absolute compiler spec.
- Treat `references/pitfalls-and-debug.md` as operational heuristics for diagnosis and remediation.
- Do not claim live synchronization with remote docs in this v1 skill.

## Source Links
- workshop.codes wiki root: https://workshop.codes/wiki
- workshop.codes article index: https://workshop.codes/wiki/articles
- md.owbastion.codes: https://md.owbastion.codes
