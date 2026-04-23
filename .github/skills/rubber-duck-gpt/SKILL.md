---
name: rubber-duck-gpt
description: Route cross-family critique requests from Claude-family primaries to the rubber-duck-gpt custom agent.
---

# rubber-duck-gpt

Use this when the primary orchestrator is Claude-family and you need a second-opinion review from the GPT-family custom agent `rubber-duck-gpt`.

Do not use this for implementation, trivial/cosmetic work, or when the primary orchestrator is already GPT/OpenAI-family. Do not use it if no concrete artifact exists yet.

## Rules

- Critique-only, read-only: no patches, file edits, shell writes, or repo changes.
- Anti-recursion: never invoke any rubber-duck skill or agent from inside the delegated review.
- Determine review mode first: `PLAN`, `CODE`, `TEST`, or `UNSTUCK`.
- Gather the artifact under review plus adjacent callers, contracts, config, and tests needed to review it.
- Delegate to the custom agent via the task/subagent flow.
- If `rubber-duck-gpt` is unavailable, stop and tell the user the GPT duck is not installed or not loaded. Do not fall back to same-family self-review.

## Delegation checklist

- Primary family confirmed: Claude-family
- Review mode chosen: PLAN | CODE | TEST | UNSTUCK
- Artifact collected: plan, diff/code, test file, or failure log
- Adjacent context collected: callers/contracts/config/tests
- Delegating to custom agent: `rubber-duck-gpt`
- Delegation prompt says: critique only, read only, no recursion

## Delegation template

Use the task/subagent flow and send the custom agent a compact brief like:

> Review mode: [PLAN|CODE|TEST|UNSTUCK]  
> Primary family: Claude-family  
> Artifact: [path/diff/log/plan]  
> Adjacent files: [paths]  
> Constraints: critique-only, read-only, no code writing, no file changes, no rubber-duck recursion.  
> Task: return a concise cross-family review with concrete findings and impact.
