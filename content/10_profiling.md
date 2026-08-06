# Profiling

Profiling and optimizing code is generally a time-consuming process, due to repeated iterations that often involve trial-and-error. Agentic AI can help speed this up by writing profiling scripts, finding hotspots, trying alternatives, and measuring the results, all with little human intervention required. What makes this easy for AI is that there is a clear goal (make it faster, use less memory, or make it more efficient in some particular way) while sticking with the design and requirements that are enforced by the test suite (see [Writing tests](./09_writing_tests.md)).

## Start profiling

All it takes is a single prompt to get started. For example, you can ask the agent to profile a function in your codebase, and it will come up with a plan for how to do it. It starts by identifying the right tool for the job (e.g. `cProfile` for Python or `perf` for compiled binaries). It handles the verbose raw output from these lower-level tools well - the kind of thing people usually skim or need a separate viewer to interpret. If a profiler is not available, or the profiler requires permissions that you don't have, it will find alternative approaches like temporarily modifying the code to identify time-consuming pieces. It can also set up a script to run through common workflows and collect the results to make them easier to analyze.

:::{caution} Verify the measurement
Check what the script actually times before trusting conclusions.
:::

## Iterate on performance

Trying out different ideas is extremely easy with agentic AI. You can try many more options, and attempt riskier or more complex ideas than you would normally do by hand, since wasteful iterations are much less costly. The agent can also suggest new approaches to improve performance, and help you avoid common pitfalls (e.g. benchmark noise, cache effects, thermal throttling). However, it is important to keep in mind that you have a better understanding of the bigger picture, so it works best when you provide guidance and constraints.

## When to profile

This will depend on your specific situation and will be up to your discretion. AI makes it easy enough that you could profile after each change, or just when the change is big enough or complex enough that you feel the need to do so.

Another common case is when you notice that something is running slower than it used to be. You can ask the agent to check older versions and compare the performance. It can then help you identify the change that caused the slowdown, and suggest ways to fix it.

## One-off vs. lasting

The one-off profiling scripts that we have mentioned are a uniquely AI-shaped task, since most people won't spend the time to write them just to profile a specific thing. However, if you find yourself profiling the same thing repeatedly, it may be worth investing in a more permanent solution. For example, you could have the agent help you build a benchmark suite (e.g. [ASV](https://asv.readthedocs.io/en/stable/)) that can be run automatically to track performance over time.

## Exercise

Profile a slow function in `agentic-ai-example`, test two optimization ideas, and report the numbers.
