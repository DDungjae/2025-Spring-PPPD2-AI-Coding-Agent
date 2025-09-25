**Programming in the AI Era — AI Agent for Code Generation and VRPTW
**
**What this repository is
**This project designs a multi-agent, LLM-routed programming system inspired by LangGraph. It is evaluated on code generation benchmarks (HumanEval) and applied to a combinatorial optimization case study, the Vehicle Routing Problem with Time Windows (VRPTW).

**Agent system overview
**We follow a waterfall-style software process (requirements, design, development, testing, deployment, maintenance) and map each stage to cooperating agents.
— problem_analysis agent: formalizes the task, specifies function signatures, and drafts pseudocode.
— development agent: writes program code from the analysis; imports and code are produced separately to reduce execution errors.
— precheck step: revises code before running.
— test_agent: executes the program and captures failing tests or runtime errors.
— reflect agent: diagnoses failures and feeds fixes back to the development agent.
Control loop: the system alternates between testing and reflection until tests pass or the iteration limit is reached, enabling autonomous error recovery with minimal human intervention.

**Benchmarks (HumanEval)
**The main metric is pass@1. The multi-agent system, even with a lightweight base model, achieves higher pass@1 than a single-model baseline. The improvement comes from role specialization, iterative reflection, and tool-use routing rather than single-shot generation.

**Case study: VRPTW
**VRPTW extends VRP by adding time windows and vehicle capacity constraints. The objective is to minimize routing cost while satisfying demand, capacity, and service-time windows.
Findings: a single LLM frequently violates constraints. The agentic system produces a merging-based heuristic that respects capacity and most time-window constraints more consistently, yielding clearer algorithmic structure, though not always the best objective value.

**Suggested repository structure
**Top-level files: README, agents, graphs for orchestration, benchmarks with a HumanEval runner, a vrptw module containing data, solver, and evaluation, and scripts for running benchmarks and the case study.

**Quick start
**Step 1. Install dependencies with your package manager using the provided requirements file.
Step 2. Run HumanEval with the multi-agent setting and your chosen model.
Example command: python benchmarks/humaneval_runner.py --agent multi --model gpt-4o-mini
Step 3. Solve a VRPTW instance.
Example command: python vrptw/solver.py --instance vrptw/data/sample.json

**Results summary
**— Higher solution rate and code quality than a single LLM on HumanEval.
— Better constraint awareness on VRPTW through agent coordination.
— Reduced human intervention due to the reflect–repair loop.

**Limitations and future work
**The VRPTW heuristic is not guaranteed to be optimal. Future work includes stronger routing policies, more reliable constraint handling, improved tool use, and automatic unit-test synthesis inside the loop.

**Acknowledgments
**Course: PPPD (Programming in the AI Era), 2025-1. This README summarizes the team’s final presentation.
