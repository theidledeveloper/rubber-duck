## Rubber Duck (Cross-Model Review)

See the canonical **Rubber Duck Protocol** in `AGENTS.md` for the full specification.

**Quick reference:**
- Claude-family orchestrating → delegate to `rubber-duck-gpt` agent (GPT-5.4)
- GPT/OpenAI-family orchestrating → delegate to `rubber-duck-claude` agent (Claude Sonnet 4.6)
- If primary family is unclear → skip until cross-family pairing can be confirmed
- Auto-invoke after: plans with tradeoffs, high-risk or multi-file/shared-contract code changes, written or heavily rewritten tests, repeated failed debug attempts
- Before raising a finding, inspect the repo if the answer is already in code/config/tests; avoid speculative findings
- Never chain duck-to-duck — review depth is 1
