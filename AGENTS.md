## Rubber Duck Protocol

Cross-model second-opinion review. A different AI family reviews the primary agent's work
at high-leverage checkpoints to catch blind spots caused by shared training biases.

### Which agent to invoke

| Primary orchestrator family | Rubber Duck agent to use |
|-----------------------------|--------------------------|
| Claude family (Opus, Sonnet, Haiku) | `rubber-duck-gpt` — pinned to GPT-5.4 |
| GPT / OpenAI family (GPT-5.x, Codex variants, GPT-4.1) | `rubber-duck-claude` — pinned to Claude Sonnet 4.6 |
| Unknown / mixed / unclear family | Skip auto-invocation until you can guarantee an opposite-family reviewer |

If the pinned model is unavailable, skip the review and note it in your response.
Never substitute a same-family model — that defeats the cross-model purpose.
If the primary family is unclear, do not guess.

### Hard recursion guard

Rubber duck agents (`rubber-duck-gpt`, `rubber-duck-claude`) must **never** invoke another
rubber duck agent. Review depth is capped at 1. If a rubber duck agent is already running,
do not chain into a second one.

### When to invoke automatically (best-effort)

Auto-invocation is advisory. Invoke when the risk/benefit ratio is high:

1. **After drafting a plan** that contains architectural tradeoffs, new abstractions, or
   significant design choices, especially when compatibility or rollout is part of the plan.
2. **After implementing** security, authentication, data schema, API contracts, database
   migrations, dependency wiring, shared config, or any multi-file change touching shared
   helpers, contracts, or generated artifacts.
3. **After writing or substantially rewriting tests** and before executing them — catch
   tautological assertions, fixture drift, and missing edge cases while fixes are cheap.
4. **After the first failed retry** on a build, test, or debug run — or when two attempts
   repeat the same theory — the primary agent may be solving the wrong problem.

Low-risk signals (skip the review): trivial single-function edits, cosmetic changes,
documentation-only, scaffolding with no logic.

### How to invoke

Delegate to the appropriate agent explicitly. Example phrasing:
> "Delegate to `rubber-duck` to critique this plan in PLAN mode."

Pass the artifact (plan text, diff, test file path) as context. The rubber duck will
read referenced files independently if paths are provided.
Before surfacing a finding, resolve any question that can be answered by inspecting the
repository. Prefer verified facts from file reads/search over speculative risks.

### How to incorporate feedback

After receiving the rubber duck's output:
1. Read every finding — do not skim.
2. For each finding, state one of: **Fixed** (describe the change), **Accepted risk**
   (document why), or **Deferred** (log for follow-up).
3. Re-invoke the rubber duck only if the verdict was 🔴 Block and fixes were made.
4. If the verdict was ✅ Clear or 🟢 Proceed, continue without re-review.

### Manual fallback

If auto-invocation did not occur and you are about to declare a high-risk or multi-file
task complete,
invoke the rubber duck manually before finishing. Tell the user you are doing so.
