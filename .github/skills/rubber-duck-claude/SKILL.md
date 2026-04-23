---
name: rubber-duck-claude
description: Route cross-family critique requests from GPT/OpenAI-family primaries to the rubber-duck-claude custom agent.
---

# rubber-duck-claude

Use this when the primary orchestrator is GPT/OpenAI-family and you need a second-opinion review from the Claude-family custom agent `rubber-duck-claude`.

Do not use this for implementation, trivial/cosmetic work, or when the primary orchestrator is already Claude-family. Do not use it if no concrete artifact exists yet.

## Rules

- Critique-only, read-only: no patches, file edits, shell writes, or repo changes.
- Anti-recursion: never invoke any rubber-duck skill or agent from inside the delegated review.
- Determine review mode first: `PLAN`, `CODE`, `TEST`, or `UNSTUCK`.
- Gather the artifact under review plus adjacent callers, contracts, config, and tests needed to review it.
- Delegate to the custom agent via the task/subagent flow.
- If `rubber-duck-claude` is unavailable, stop and tell the user the Claude duck is not installed or not loaded. Do not fall back to same-family self-review.

## Delegation checklist

- Primary family confirmed: GPT/OpenAI-family
- Review mode chosen: PLAN | CODE | TEST | UNSTUCK
- Artifact collected: plan, diff/code, test file, or failure log
- Adjacent context collected: callers/contracts/config/tests
- Delegating to custom agent: `rubber-duck-claude`
- Delegation prompt says: critique only, read only, no recursion

## Delegation template

Use the task/subagent flow and send the custom agent a compact brief like:

> Review mode: [PLAN|CODE|TEST|UNSTUCK]  
> Primary family: GPT/OpenAI-family  
> Artifact: [path/diff/log/plan]  
> Adjacent files: [paths]  
> Constraints: critique-only, read-only, no code writing, no file changes, no rubber-duck recursion.  
> Task: return a concise cross-family review with concrete findings and impact.
