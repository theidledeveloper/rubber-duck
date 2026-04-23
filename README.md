# Rubber Duck

> A reusable GitHub Copilot CLI package for cross-family, critique-only review.

Rubber Duck gives your main coding agent a second opinion from a different model family before a risky plan, change, test rewrite, or debug loop goes any further.

This repo is not a general app. It is a small, reusable package of custom agent definitions and optional skill entry points that you can keep in this repo, copy into another repo, or install for personal use.

## What Rubber Duck does

Rubber Duck reviews artifacts such as:

- plans with real tradeoffs
- multi-file or high-risk code changes
- newly written or heavily rewritten tests
- repeated failure/debug loops

It is intentionally narrow:

- **critique-only**
- **read-only**
- **no recursion**
- **3-7 findings max**
- **explicit verdict every time**

The goal is not more output. The goal is sharper output: concrete findings, concrete impact, and a clear verdict.

## How Rubber Duck works

Rubber Duck is built around one rule: the reviewer should come from a different model family than the primary session.

### Cross-family rule

- **GPT / OpenAI-led session** → use **`rubber-duck-claude`**
- **Claude-led session** → use **`rubber-duck-gpt`**

If you cannot tell which family is leading the session, do not guess. Pick the reviewer only when the opposite family is clear.

### Review modes

- **PLAN**: challenge abstractions, missing requirements, hidden dependencies, and scope drift
- **CODE**: catch contract drift, silent breakage, boundary mistakes, and missing error paths
- **TEST**: catch weak assertions, fixture drift, and missing regression coverage
- **UNSTUCK**: break repeated-debug loops by questioning the assumed root cause

## Why this repo has both skills and agents

Rubber Duck uses two layers because they solve different problems.

### Agent layer

Custom agents are the actual review endpoints. They contain the strict behavior and guardrails: critique-only, read-only, no recursion, capped findings, and explicit verdicts.

You can invoke them directly when you know exactly which duck you want.

### Skill layer

Skills are the user-facing entry points. In Copilot CLI, skills can be auto-selected when relevant, and you can also target one explicitly with `/skill-name`.

That makes skills useful for discoverability and convenience, while the custom agents remain the authoritative review definitions underneath.

In short:

- **agents** = the review policy and behavior
- **skills** = the entry points that make that behavior easier to use

## Repository layout

| Path | Type | Purpose |
| --- | --- | --- |
| `.github/agents/rubber-duck-claude.agent.md` | Custom agent | Claude-family reviewer for GPT/OpenAI-led sessions |
| `.github/agents/rubber-duck-gpt.agent.md` | Custom agent | GPT-family reviewer for Claude-led sessions |
| `.github/skills/rubber-duck-claude/SKILL.md` | Project skill | Optional project-local skill entry point for the Claude duck |
| `.github/skills/rubber-duck-gpt/SKILL.md` | Project skill | Optional project-local skill entry point for the GPT duck |

## Install

GitHub Copilot CLI supports both project-local and personal install locations:

- **Project skills**: `.github/skills/<skill-name>/SKILL.md`
- **Personal skills**: `~/.copilot/skills/<skill-name>/SKILL.md`
- **Repo custom agents**: `.github/agents/*.agent.md`
- **User custom agents**: `~/.copilot/agents/*.agent.md`

### 1) Use this repo as-is in its own project scope

Use this when this repository itself is your active Copilot project.

1. Keep the custom agents in:
   - `.github/agents/rubber-duck-claude.agent.md`
   - `.github/agents/rubber-duck-gpt.agent.md`
2. Add the project-local skills in:
   - `.github/skills/rubber-duck-claude/SKILL.md`
   - `.github/skills/rubber-duck-gpt/SKILL.md`
3. Open Copilot CLI in this repo and invoke either the custom agents directly or the skills, for example `/rubber-duck-claude`.

### 2) Copy Rubber Duck into another repository

Use this when you want Rubber Duck available only inside a different project.

1. Copy these agent files into the target repo:
   - `.github/agents/rubber-duck-claude.agent.md`
   - `.github/agents/rubber-duck-gpt.agent.md`
2. Copy these skill files into the target repo if you want skill-based entry points:
   - `.github/skills/rubber-duck-claude/SKILL.md`
   - `.github/skills/rubber-duck-gpt/SKILL.md`
3. In the target repo, use the opposite-family duck for the current session.

### 3) Install Rubber Duck for personal use

Use this when you want the same ducks available across many repos.

1. Copy the custom agents to:
   - `~/.copilot/agents/rubber-duck-claude.agent.md`
   - `~/.copilot/agents/rubber-duck-gpt.agent.md`
2. Copy the skills to:
   - `~/.copilot/skills/rubber-duck-claude/SKILL.md`
   - `~/.copilot/skills/rubber-duck-gpt/SKILL.md`
3. Invoke the custom agents directly, or call the skills explicitly with `/rubber-duck-claude` or `/rubber-duck-gpt`.

## How to use it

### Direct agent usage

Use the custom agent directly when you know which duck you need. In Copilot CLI, that can be a natural-language request that names the agent, or a CLI invocation that targets the agent explicitly.

#### In a GPT / OpenAI-led session

Use the Claude duck:

```text
Use the rubber-duck-claude agent to review this plan in PLAN mode. Focus on
missing requirements, dependency surface, and multi-file coordination. Return
at most 5 findings and a verdict.
```

```text
copilot --agent=rubber-duck-claude --prompt "Review this diff in CODE mode. Look
for contract drift, silent failures, wiring breakage, and missing error handling."
```

#### In a Claude-led session

Use the GPT duck:

```text
Use the rubber-duck-gpt agent to review this test file in TEST mode. Focus on
tautological assertions, fixture drift, and missing regression coverage.
```

```text
copilot --agent=rubber-duck-gpt --prompt "We are stuck after two failed fixes.
Review this error log in UNSTUCK mode and name the repeated assumption before
proposing alternatives."
```

### Skill usage

If you install the skill layer, Copilot can auto-select the skill when it is relevant, and you can also call it explicitly with `/skill-name`.

Examples:

```text
/rubber-duck-claude Review this migration plan.
```

```text
/rubber-duck-gpt Critique this diff before I call it done.
```

## When to use Rubber Duck

Use it when the cost of being wrong is higher than the cost of one more review pass:

- before committing to a plan with real architectural consequences
- after a change that touches multiple files, contracts, config, or wiring
- after rewriting tests that could give false confidence
- when a bug survives multiple attempts and the team may be stuck on the wrong theory

Do not use it for trivial edits, cosmetic changes, or unfinished work that is still missing the artifact to review.

## Short references

- GitHub Copilot custom agents: `.github/agents/*.agent.md` and `~/.copilot/agents/*.agent.md`
- GitHub Copilot skills: `.github/skills/<skill-name>/SKILL.md` and `~/.copilot/skills/<skill-name>/SKILL.md`
