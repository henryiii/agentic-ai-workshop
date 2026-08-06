# Setup

Instructions to set up your environment for the workshop.

## Workshop for today

We'll be running in the same JupyterLab instance we've been using. Since these tools are CLIs, we can run them inside JupyterLab's terminal.

We'll be running OpenCode and the Nemotron 3 Super 120B model. You'll need to set up the harness. We've provided a setup file at `/u0b/software/training/opencode.jsonc` that you can copy to `~/.config/opencode/opencode.jsonc`. The contents of the file are:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "sdcc": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "SDCC",
      "options": {
        "baseURL": "https://inference0-api.sdcc.bnl.gov/v1",
        "apiKey": "{env:OPENAI_API_KEY}"
      },
      "models": {
        "nemotron-3-super-120b": {
          "name": "nemotron-3-super-120b",
          "limit": {
            "context": 131072,
            "output": 32768
          }
        }
      }
    }
  }
}
```

You'll need to set `OPENAI_API_KEY` to the key you were provided for the workshop. You can also replace the `{env:OPENAI_API_KEY}` string with the contents of the key.

## Code

This repository is <https://github.com/henryiii/agentic-ai-workshop>, which holds the workshop material only. You want <https://github.com/henryiii/agentic-ai-example>, which is the example project we'll be working on. It was created with this prompt in Claude Fable 5:

### Input

> This is an example repo for a workshop. I want to start by implementing a library: let's make a terminal-based plot tool, fairly similar to matplotlib, but for the terminal. We should make components for axis (doing tick marks is the hard part of plotting libraries, and we are more restrictive than normal being in a terminal with fixed chars!). We can do three plots for now: a standard line plot, a bar plot, and a 2d "imshow" style plot, using terminal colors. We can use dual colors per bin on that one; terminals have a half-height block and we can use bg/fg colors. A few guidelines:
>
> - We can make a demo app with textual (similar to <https://github.com/scikit-hep/uproot-browser> but of course minimal with sample plots and a couple of controls to change the plots) where students can see the plots. This isn't part of the core library, but an `example/app.py` and textual can be in our dependency-groups in examples -> dev.
> - This is a demo repo, so don't worry about fine edge cases, simple code is better than perfectly correct code under all circumstances. If it adds lots of code to handle NaNs or something, go with the simpler code.
> - Try to use high level OO design, don't worry about exact API matching with anything (Matplotlib's OO API if you do need a target, but simpler is fine)

### Output

The model reported:

> Done — the terminal plotting library is implemented, tested, and lint-clean. All 35 tests pass, prek (ruff, mypy, codespell, etc.) is clean, and pylint scores 10/10.
>
> **What was built**, as flat modules in `src/agentic_ai_example/`:
>
> - `canvas.py` — `Canvas`, a grid of `Cell`s (char + optional fg/bg RGB). `put()` silently clips out-of-range writes so artists need no bounds checks. `render()` emits ANSI truecolor codes only when the color changes, with resets so color never leaks across lines.
> - `ticks.py` — the teaching centerpiece: `nice_ticks()` (MaxNLocator-style snap to 1/2/2.5/5/10 × power of ten, snapping up so the count never exceeds what fits) and `format_ticks()` (one shared format per axis so label widths are predictable).
> - `colormap.py` — `Colormap` (linear interpolation over RGB control points) and a 5-point `VIRIDIS`.
> - `artists.py` — `Transform` (data → cell coordinates, y flipped), plus `Line` (● markers connected cell-by-cell), `Bars` (full blocks with ▁▂▃▄▅▆▇ partial tops), and `HeatMap` (two pixels per cell via ▀ with fg/bg colors).
> - `figure.py` — `Axes` (collects artists, unions their bounds, computes the y-label gutter after picking ticks so labels always fit, greedy-drops overlapping x labels) and `Figure` (`print(fig)` works via `__str__`).
>
> The placeholder `example.py` is deleted, and `__init__.py` exports `Figure`, `Axes`, `Colormap`, `VIRIDIS`. The library is stdlib-only.
>
> **Demo app**: `examples/app.py` is a ~80-line Textual app (textual lives in a new `examples` dependency group, included in `dev`). Keys 1/2/3 switch line/bar/heatmap, +/− adjust a parameter, q quits; plots resize with the terminal. I verified it headlessly with Textual's test pilot. Run it with `uv run --group examples python examples/app.py`.
>
> Two small deviations from the plan worth noting: I allowlisted `bar` in the pylint config (it's in pylint's default placeholder-name blocklist, but it's the correct API name), and one test assertion was wrong on first run (expected a "3" y-label where `nice_ticks(0, 3, 3)` correctly yields [0, 2]) — the render itself was right.
>
> I've left the changes uncommitted; let me know if you'd like a commit.

### Followup

This is mostly the output (including a couple of bugs you'll be finding), but I did follow up with "Is there a good way to make the textual example app more friendly to running headless, so a harness can control it and look at the output?" and guided it into making a better headless setup to run your AI harness on. Claude Code was happy enough without it, but this helps the smaller models with something they can run without getting stuck waiting for keypresses. This mostly added `examples/headless.py` and `tests/test_app.py`.

## Outline for general use

If you are not part of the workshop, here's an outline of the general setup steps.

- Pick a harness (all handle similarly, with common commands)
  - Three flavors: TUI (works anywhere, incl. clusters), GUI apps, editor plugins
    - Most harnesses come in all three; pick the one you like
      (Copilot CLI, Claude Code, Codex, OpenCode, Pi, Cursor, Muse Code, etc.)
  - The system prompt is the biggest difference between harnesses running the same model
    - Some providers (Anthropic) require their harness for subscription use
    - Prompt caching varies between providers (Claude is 1 hour, Codex 1 week, etc.)
  - Trade-offs: permission models, system prompt size, configurability
- Pick a model
  - Model tiers: frontier / workhorse / simple / local
    - questions and throwaway scripts → local
    - triage and lint fixes → simple
    - review, bugfixes, conversion → workhorse
    - refactors, features, profiling → frontier
  - Start with a good model so first attempts succeed
  - Effort levels; a strong model on high effort can overengineer simple tasks
  - Cheaper/faster and local models come later, once you can judge task difficulty
- Accounts and access
  - Copilot Pro / free tiers, education accounts, OSS subscriptions
  - Optional: local models (memory requirements, quantization in brief)
- Install and sign in
  - Download the harness, sign in or set the API key
  - Create a user-level config file (`~/.claude/CLAUDE.md`, `~/.config/opencode/AGENTS.md`,
    `~/.pi/agent/APPEND_SYSTEM.md`) — template covered in the AGENTS.md chapter
- Security basics before you start
  - Permission modes: manual approve vs. auto-approve; containers and codespaces as isolation
  - Do not give the agent access to anything it can destroy (databases, tokens)
- Clone the workshop example repository (`agentic-ai-example`, a small plotting package)
  and verify your harness runs in it
- Optional: usage tracking across harnesses (`uvx agentsview`, `npx ccusage`)
