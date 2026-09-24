# Agent team for Mona's Project Pulse dashboard

This custom team will be used to build Mona's Project Pulse dashboard with GitHub Copilot CLI orchestrating the work inside a Codespace.

## Team members

- Planner — Model: Claude Opus 4.7 (copilot)
  - Responsibility: researches the repository, reads relevant docs and files, identifies edge cases, and produces an implementation plan for the orchestrator.
  - Definition: `.github/agents/planner.agent.md`

- Orchestrator — Model: Claude Opus 4.7 (copilot)
  - Responsibility: breaks the work into phases, delegates tasks to specialists, manages dependencies, and keeps the integration coherent.
  - Definition: `.github/agents/orchestrator.agent.md`

- Designer — Model: Gemini 3.1 Pro (copilot)
  - Responsibility: designs the dashboard UX and visual structure, focusing on usability, accessibility, hierarchy, layout, and responsive behavior for Project Pulse.
  - Definition: `.github/agents/designer.agent.md`

- Coder — Model: GPT-5.5 (copilot)
  - Responsibility: implements the application logic and code changes in the assigned file scope, validates the behavior, and supports runnable app setup when needed.
  - Definition: `.github/agents/coder.agent.md`

## How we will work

Using GitHub Copilot CLI in a Codespace, the Orchestrator will coordinate the Planner, Designer, and Coder agents so the dashboard work is split into clear responsibilities and validated as a complete result.
