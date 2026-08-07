# Repetitive work

How to apply the same change many times, and when to write a script instead.
Like the last chapter, the quoted prompts are real prompts from real
sessions, with links to the resulting PRs where public.

## Repeating your own work

Do the first instance by hand, then have the agent apply the pattern to the
rest. You set the quality bar with your example; the agent does the
repetition. Often you do not even have to describe the pattern:

> A review of #1268 noted that InvalidMetadata is unpicklable on main. Are
> there any other unpicklable classes we might want to make picklable?

— [packaging#1328](https://github.com/pypa/packaging/pull/1328)

The fix for one class was in the PR already; this prompt turned it into a
sweep over the whole package. The generalizing question is a habit worth
building -- after any targeted fix, ask:

> Yes. Are there any others that should be sparse aware?

## Repetitive maintenance tasks

Dropping an old language version is the classic example: the version appears
in classifiers, CI matrices, docs, `requires-python`, and conditional code,
and dropping it unlocks modernization (like pattern matching) everywhere.
Agents are also happy to modernize in general:

> Cleanup and modernize @scripts/ExtractReleaseNotes.py - use argparse, raise
> SystemExit, etc.

— [CLI11#1402](https://github.com/CLIUtils/CLI11/pull/1402)

> Can we keep it a method, but use pattern matching in it?

— part of [cookie#833](https://github.com/scientific-python/cookie/pull/833)

Combine with linters and CI so nothing is missed: Ruff's `UP` rules catch old
idioms mechanically, and a CI job pinned to your minimum version catches what
the sweep skipped. The repetition can also fan out -- one subagent per file or
per item scales the same instruction across a codebase:

> Let's try to find all the places the docs are missing
> versionadded/versionchanged. Launch opus agents on each Python file, find
> when things were added or changed behavior, and then make sure docstrings
> and docs have versionadded/versionchanged as needed.

— [packaging#1344](https://github.com/pypa/packaging/pull/1344)

> The older C++ posts are less detailed than 26 now. Can we check the
> snippets, add paper links, and fill out missing features? (maybe a subagent
> for each?)

## Adding static types: a special case

Typing an existing codebase is slow by hand, and non-AI automation
(`MonkeyType`, stub generators) does a poor job. This shape of work is ideal
for agents: highly repetitive, locally verifiable (the type checker is a
pass/fail oracle), and occasionally requiring real understanding. Even a weak
model handles the first 80% of annotations; then a strong model hunts down
the remaining `Any`s, `type: ignore`s, and the places where the code needs a
small refactor to be typeable at all. Turn the checker's strictness up as you
go so finished files stay finished. The same recipe works across languages --
JavaScript to TypeScript is the same task with a different checker.

## Capture the process: `SKILL.md`

If you repeat a task across sessions or repos, write it down as a skill: an
open standard for human- and AI-readable instructions, invoked explicitly
(`/drop-python`) or matched automatically from its description. A skill can
carry helper scripts and reference files along with the instructions:

> Let's make a new skill for exploring issues in a repo. It should have the
> @scripts/github-issues-to-sqlite.py in its scripts; it should download the
> issues locally before exploring.

— now in [henryiii/skills](https://github.com/henryiii/skills)

Examples that work well: drop-a-Python-version, add a minimum-version CI
job, apply repo-review, prepare a changelog in your house style. Use the AI
to help write and maintain them -- after a session where you corrected the
agent, fold the corrections back in:

> Can you update the skill we used to include these extra greps?

Skills also give project knowledge a better home than an ever-growing
`AGENTS.md`, since they only load when relevant (see [](02_agents_md.md)):

> Let's move the mention in AGENTS about releases to a skill (in
> `.agents/skills`, we can symlink `.claude/skills` there).

— [CLI11#1407](https://github.com/CLIUtils/CLI11/pull/1407)

## When to write a script instead

If the transform is deterministic and high-volume, do not have the agent
perform each instance; have it write a script, then run the script. That is
fewer tokens, faster, reviewable, and exactly reproducible -- an agent editing
100 files by hand will eventually get creative as the context window fills up.

> I want to write a script that pulls the referenced files and puts them
> somewhere, so we can analyse them later. Would putting them in sqlite make
> sense? Or something else? What's the best way to get them with GH API
> limits?

The rule of thumb: agents for judgment, scripts for mechanics. The agent is
good at writing the script and judging the edge cases the script reports.

## Exercise

:::{exercise}
Write a small `SKILL.md` for a task you repeat, and apply it to
`agentic-ai-example`. If nothing comes to mind, capture your changelog or
release-notes style as a skill.
:::
