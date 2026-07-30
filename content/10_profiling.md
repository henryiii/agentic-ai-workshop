# Profiling

How to measure performance and let an agent read the results.

## Outline

- Ask for a profile: the agent writes a profiling script on the spot
- Iterate on performance
  - Ask it to find ways to make it faster, or to try your specific ideas
  - Re-profile after each change and compare
- Compare across versions
  - It can check out older versions of the library and measure those too
- One-off vs. lasting
  - One-off profiling scripts are the uniquely AI-shaped task
  - For something permanent, have it help you build a benchmark suite (e.g. ASV)
- Caution: verify the measurement — check what the script actually times before trusting conclusions
- Exercise: profile a slow function in `agentic-ai-example`, test two optimization ideas,
  and report the numbers
