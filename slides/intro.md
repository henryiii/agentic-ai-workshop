---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

# Agentic AI Workshop

## Practical agentic coding for developers

Henry Schreiner

Materials: <https://github.com/henryiii/agentic-ai-workshop>
Example repo: <https://github.com/henryiii/agentic-ai-example>

---

## Today (1.5 hours)

- What an agent is, and the one core skill: **context management**
- `AGENTS.md`: teaching the agent your repo
- Hands-on: inspection, review, triage, CI fixes
- Everyday wins: common tasks, repetitive work, tests
- Bigger jobs: profiling, new features, refactors

Interleaved short exercises on `agentic-ai-example`,
a small terminal plotting library (itself AI-generated).

---

## Setup for today

Run in the JupyterLab terminal: **OpenCode** + **Nemotron 3 Super 120B**

1. Copy the provided config:
   `/u0b/software/training/opencode.jsonc` → `~/.config/opencode/opencode.jsonc`
2. Set `OPENAI_API_KEY` to your workshop key
3. Clone the example project:

```console
$ git clone https://github.com/henryiii/agentic-ai-example
$ cd agentic-ai-example && opencode
```

---

## Setup in general (after today)

- **Pick a harness**: TUI / GUI / editor plugin
  (Claude Code, Codex, OpenCode, Copilot CLI, Pi, Cursor, …)
- **Pick a model** by tier — start strong, go cheaper later:
  - local/simple → questions, triage, lint fixes
  - workhorse → review, bugfixes, conversions
  - frontier → refactors, features, profiling
- **Security**: never give the agent access to anything it can destroy

---

<!-- _class: lead -->

# Intro to Agentic AI

---

## It's useful now?

November 2025: harnesses (Claude Code) + models (Opus 4.5)
turned AI from "occasionally helps" into a real tool.

Two things get lumped together as "agentic AI":

- **A developer driving an interactive harness** ← this workshop
- **Autonomous agents** running unattended on cheap models

Autonomous agents flooding open source: OpenClaw repo went from
~2 PRs/week to ~3,400/week; merge rate 48% → under 10%.

**AI scales up impact — for good or bad.**

---

## Chat model vs. agent

- LLM: predicts the next token (completion engine)
- Chat model: + fine-tuning and RLHF (human preferences)
- Agentic model: + tool use, thinking, **RLVR** —
  rewarded when solutions pass verifiable checks (test suites)

Agents run a **loop**:

```
think → tool call (edit, run, grep) → inspect output → repeat
```

They compile, lint, test, and iterate until it works.

---

## Vocabulary

| Term | Meaning |
| --- | --- |
| token | ~¾ of a word; the unit models read/write |
| context window | everything the model sees at once (200K–1M) |
| harness | the program around the model: prompt + tools |
| tool | a function the model calls (read, edit, bash) |
| MCP | open standard to plug in external tool servers |
| subagent | fresh-context helper that reports a summary back |
| skill | packaged instructions, often a slash command |

---

## Good at / not good at

**Good at:**

- Clear **pass/fail** conditions: tests, linters, CI feed the loop
- Long or annoying tasks (slow CI, Docker, obscure logs)
- Mimicking your existing code and style

**Weaker at:**

- Judgment calls — only as good as what's in context
- Anything past the training cutoff (load it into context!)
- From-scratch code; passing off its text as human writing

---

## The core skill: context management

- **Too little context** → bad decisions.
  Load design docs, issues, related files.
- **Too full context** → forgetful.
  Advertised 1M ≠ usable; stay under ~300–400K.
- `/compact` summarizes the conversation and frees space —
  do it at natural breaks, and when resuming old sessions.

---

## Working norms and disclosure

- Mark AI text clearly: `:robot: _AI text below_ :robot:`
- Credit in commits with the Linux kernel trailer:
  `Assisted-by: <harness>:<model>`
  — never `Co-authored-by`, **never** `Signed-off-by` (DCO is legal)
- Disclose in PR descriptions; follow project `AI_POLICY.md` if present
- Golden rule (LLVM/curl): a contribution must be worth more
  than the time it takes to review
- **Never** use AI on "good first issues"

---

<!-- _class: lead -->

# AGENTS.md

---

## The first thing to do in any repo

`AGENTS.md` at the top level gives the agent:

- High-level overview and light structure
- Common tasks: how to run tests, linters
- Conventions to follow (`uv run`, changelog rules, …)

Run `/init` to generate it; then edit (or iterate with AI).

**Keep it short** — it's read every session, so it costs context
every run. Move sometimes-needed info to separate files.

Claude Code quirk: `ln -s AGENTS.md CLAUDE.md`

---

## Commit it or not? Plus user config

Three camps: **commit it** (shared conventions), **gitignore it**,
or keep it fully private via `.git/info/exclude`.

You also get a **user-level** file (`~/.claude/CLAUDE.md`,
`~/.config/opencode/AGENTS.md`) for *your* conventions:

- Who you are (GitHub username), your tool preferences
- "Write the regression test first"
- AI markers and commit trailers
- Output style tweaks

---

## Exercise 1

Run `/init` on `agentic-ai-example`.

Review the result: is it short? Is anything wrong?
Refine it by hand or by prompting.

---

<!-- _class: lead -->

# Inspection

---

## Ask questions of a codebase

> How do I run the tests?

> Where is X defined? *(works across languages — try CPython!)*

> Can this return value be `None`?

> Who wrote this function?

> Write an `ARCHITECTURE.md` describing how this project works

Watch it work: it greps, reads, and traces much like you would.
Works beyond code too: diff two logs, compare archives, check a webpage.

**Exercise 2:** investigate `agentic-ai-example` — how does it work?

---

<!-- _class: lead -->

# Reviewing

---

## Reviewing text and PRs

Almost anything can be reviewed — docs, plans, proposals:

> You are an NSF reviewer. What is the weakest part of this proposal?

Don't mix asks: "review this and also check links" → only links.

For PRs:

- Built-in `/review` is a great start
- **Adversarial review** (real prompt from nox#1131):

> You are an adversarial reviewer for the new feature in this branch.
> Do you see any problems? Can you break it?

---

## Reviewing AI with AI

**Rubber Duck**: implementer model + reviewer model
from *different* families of similar strength.

- GitHub: Sonnet 4.6 reviewed by GPT-5.4 closed **74.7%**
  of the gap to Opus 4.6 alone
- Matching models just praise each other

Human in the loop: **review is the bottleneck now**, not writing.
The new bottleneck behind that: **design** — do you want to
support this API forever?

---

<!-- _class: lead -->

# Issue triage

---

## Let the agent survey the tracker

> Categorize all open issues. Highlight ones that are easy to close,
> and bugs that you can reproduce

- It figures out `gh` itself; you close/respond by hand
- Great at producing **MWEs** → turn them into regression tests
- Scale up: subagents fix reproduced bugs, draft PR each

⚠️ Issue text can carry **prompt injection** — never let reporter
text drive an agent that acts without human review.

**Exercise 3:** triage `agentic-ai-example`'s issues into a ranked list.

---

<!-- _class: lead -->

# CI and bugfixes

---

## Making checks pass

> Fix the failing CI on this PR

⚠️ **The cheapest way to pass a check is to weaken it.**
Watch for skips, loosened tolerances, deleted assertions —
read the diff, not just the green mark.

It handles `git bisect` too:

> Find the commit where `test_foo` started failing

Turn issue lists into fix PRs — review the *fix*, not the problem.
Regression test first, then fix.

---

<!-- _class: lead -->

# Common tasks

*All quoted prompts are real, most link to merged PRs.*

---

## Friction, not difficulty

Small edits, fire and forget:

> Let's bump the upper limit on cmake to 4.4

Merge conflicts — often one word:

> rebase

Conversions:

> Let's convert the gitbook to mystmd — *CLI11#1389*

---

## More everyday wins

Lints (win-win: cleanup now, honest agents later):

> Can we also lint with prek on PRs?

Docs drift:

> cp314-pyodide_wasm32 is missing in the docs. Are there any
> others? — *cibuildwheel#2947*

Release chores:

> Fill out the changelog since last release

---

## Reviving old work and the backlog

> Let's rebase #4272 and see if we can make it work.
> — *pybind11#4272, opened 2022, merged 2026*

The backlog items never worth an afternoon are now worth a sentence:

> Let's remove the "needs-changelog" label infrastructure, it's not
> really needed anymore — *boost-histogram#1164*

Plus: one-off scripts, plots, TODO-filling in code and prose.

---

<!-- _class: lead -->

# Repetitive work

---

## Repeat a pattern, capture the process

- Do the first instance by hand → agent applies it to the rest
- Ask the **generalizing question** after any fix:

> Are there any other unpicklable classes? — *packaging#1328*

- Fan out with subagents; pair with linters and CI
- **Static types**: the ideal shape — repetitive, checker-verified
- Repeated across sessions? Capture it as a **`SKILL.md`** (`/drop-python`)

**Agents for judgment, scripts for mechanics** — deterministic
high-volume transforms should be a generated script.

---

<!-- _class: lead -->

# Writing tests

---

## Tests feed the loop

- **TDD works great**: your tests specify the interface;
  the agent iterates until they pass
- **Coverage-driven**: fill gaps from the report —
  beware locking in bugs from untested code
- Improve the suite: test doc examples; issue reproducers
  as regression tests (confirm they fail first!)

⚠️ Passing ≠ correct; 100% coverage ≠ tested; agents may
**weaken tests to pass** — tell them not to modify tests.

**Exercise 4:** improve coverage of `agentic-ai-example`.

---

<!-- _class: lead -->

# Profiling

---

## Cheap iterations, clear goal

Clear goal ("make it faster") + tests enforcing correctness.

- One prompt: it picks the tool (`cProfile`, `perf`)
  and digests the verbose output you'd normally skim
- Try riskier ideas — wasted iterations are cheap
- Regression? Have it compare against old versions

⚠️ **Verify the measurement** — what does the script actually time?

Profiling the same thing repeatedly? Build a benchmark suite (ASV).

---

<!-- _class: lead -->

# New features

---

## Plan first

Where AI most easily goes astray: no tests constrain new code.

- Use **plan mode**: agent explores and proposes, edits nothing
- **Edit the plan** — it's the spec; a minute of editing saves
  a wasted implementation; checklists make it resumable
- **Mix tiers**: strong model plans, workhorse implements
- **Save the plan** on the issue/PR — collaborators object early,
  reviewers check code against stated intent

---

## Implementing from a specification

Specs (PEPs, RFCs, design docs, reference implementations)
constrain the agent the way tests do in TDD:

> Now that we have a new Python discovery system, we should be able to support the requires-python option of PEP 723. — *nox#1142*

> Let's add support for dynamic-metadata. It is described in
> ../../scikit-build/dynamic-metadata and implemented in
> ../../scikit-build/scikit-build-core.

The more precise the source material, the less supervision needed.

---

## Prototype, validate, and the real cost

- **Prototype to decide**: keep it, polish it, or throw it away —
  all three outcomes are wins
- **Validate beyond your repo**: convert a downstream project;
  have the agent try to break it; have it follow your docs as a new user
- **The real cost is after generation**: review is the bottleneck;
  iterate in small pieces. *If you wouldn't merge it from a human,
  don't merge it from the agent.*

**Exercise 5:** plan and add a scatter artist or log-scale axis
to `agentic-ai-example`.

---

<!-- _class: lead -->

# Refactors

---

## Structural change, safely

Refactors that stalled for years are now a plan file away.
Your question: **is the new structure actually better?**

- Safety nets make it possible: tests, linters, type checker —
  the refactor is only as safe as the checks it must pass
- Too big for one PR? **Stage it** — each phase verified before the next

> Let's break out into 5 other PRs… — *iminuit#1145–1149*

- Run several at once in `git worktree` checkouts
- **Know your git**: rebases, force-pushes, reflog — you direct it

---

<!-- _class: lead -->

# Wrap-up

---

## Takeaways

1. **Context management** is the skill — load what's needed, compact when full
2. **Pass/fail feeds the loop** — tests, linters, and CI multiply agent value
3. **`AGENTS.md` first** in every repo; keep it short
4. **Review is the bottleneck** — and agents help there too
5. **Disclose**: mark AI text, `Assisted-by:` trailers, golden rule
6. Agents for judgment, **scripts for mechanics**

Materials: <https://github.com/henryiii/agentic-ai-workshop>

Questions?
