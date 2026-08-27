# Agent team

For Mona's Project Pulse dashboard, I will use a compact custom agent team orchestrated through GitHub Copilot CLI in a Codespace. The workflow is led by an Orchestrator that breaks the work into phases, delegates to specialist agents, and verifies that the pieces fit together cleanly.

- Orchestrator — Target model: Claude Opus 4.7 (copilot). Responsible for coordinating the Planner, Coder, and Designer agents, creating execution phases, assigning file scope, and validating the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Target model: Claude Opus 4.7 (copilot). Responsible for researching the repository, reviewing documentation and dependencies, identifying edge cases, and producing an implementation plan with file assignments and sequencing. Definition: `.github/agents/planner.agent.md`.
- Coder — Target model: GPT-5.5 (copilot). Responsible for implementing the app logic, fixing issues, and writing code within the file scope assigned by the Orchestrator, including runnable app support when required. Definition: `.github/agents/coder.agent.md`.
- Designer — Target model: Gemini 3.1 Pro (copilot). Responsible for UI/UX direction, accessibility, information hierarchy, interaction flow, and visual polish for the Project Pulse dashboard experience. Definition: `.github/agents/designer.agent.md`.

All four agents live under the repository's `.github/agents/` folder and work together through GitHub Copilot CLI in a Codespace to keep planning, implementation, and design tightly aligned.
