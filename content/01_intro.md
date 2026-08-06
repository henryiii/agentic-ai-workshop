# Intro to Agentic AI

What an agent is, how it differs from a chat model, and what it can do in a repository.

## Outline

- The shift: agentic AI became genuinely useful for real software work (November 2025)
  - Hype vs. reality; "AI won't replace you. A person using AI will."
  - Two very different things get lumped together as "agentic AI":
    a developer driving an interactive harness and taking responsibility (a power tool),
    vs. low-cost models unattended, mass-producing PRs ("AI slop")
- Chat model vs. agent
  - Chat: text in, text out; you copy-paste
  - Agent: an LLM in a loop with tools — it edits files, runs tests, reads logs, iterates
  - Agentic behavior tests theories and rejects fixes that do not work (limits hallucination)
- Core concepts (glossary)
  - LLM, tokens, context window, `/compact`, cache, training cutoff
  - Harness: system prompt + tools + MCP + agents + skills
  - Subagents: their own context window, report back a summary; forks share the parent context
  - Local and open-weight models: quantization, model size, MoE (brief)
- What agents are good at: the key themes
  - Anything with a clear pass/fail condition — tests, linters, type checkers, and CI
    feed the agentic loop and directly improve output quality
  - Things humans do not like to spend time on; long or annoying tasks (slow CI, Docker, builds)
  - Preview of the workshop chapters: inspection, review, triage, bugfixes, repetitive work, tests, features
- What agents are not good at
  - Best practices unless told; anything past the training cutoff unless in context
  - Decision making and judging what is "best" — weaker than pass/fail checking
  - Writing new code from scratch is one of the *hardest* tasks; do not start there,
    and do not delegate what you enjoy
  - Communicating for you — write your own PR descriptions
- The core skill: context management
  - Bad decisions usually mean missing context — load design docs, issues, files
  - Forgetful mid-task means context too full — `/compact`, avoid junk output
- Working norms and disclosure
  - Credit AI with the Linux kernel trailer `Assisted-by: <harness>:<model>`;
    never `Co-authored-by` or `Signed-off-by` (DCO is a human legal act)
  - Full disclosure recommended — it helps reviewers (e.g. pick a different model family);
    but conventions vary (Kubernetes forbids trailers, wants PR-description disclosure)
  - `AI_POLICY.md`: projects publish a stance (all-in / moderate / minimal);
    always follow the policy of the project you contribute to
  - The golden rule (LLVM/curl): a contribution should be worth more than the time it takes to review
  - Do not use AI on "good first issues" — they exist to teach humans
  - Review time is the new bottleneck; human review time is the most valuable resource

Link: <https://platform.openai.com/tokenizer>
