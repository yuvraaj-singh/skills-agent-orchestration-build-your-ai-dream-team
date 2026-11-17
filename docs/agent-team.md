For Mona's Project Pulse dashboard, the team is a small set of custom specialist agents defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 5.5 (copilot). Responsible for researching the repository, checking dependencies and edge cases, and producing a practical implementation plan for the dashboard work. Definition: `.github/agents/planner.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsible for UI/UX direction, accessibility, layout hierarchy, interaction flow, and polished dashboard styling for Project Pulse. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing the actual code, fixing logic bugs, and creating runnable app support files when needed for the Project Pulse dashboard. Definition: `.github/agents/coder.agent.md`.
- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsible for breaking the request into phases, delegating work to the specialist agents, coordinating file ownership, and verifying the integrated result before reporting back. Definition: `.github/agents/orchestrator.agent.md`.

This setup uses GitHub Copilot CLI in a Codespace to orchestrate the work, with the Orchestrator coordinating the Planner, Designer, and Coder agents to turn the dashboard brief into a working implementation.