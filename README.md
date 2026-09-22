# 🤖 AI Context Framework Templates


| 📝 License | 🤝 Contributions | 🤖 Target |
| :--- | :--- | :--- |
| ` MIT ` | ` PRs Welcome ` | ` AI Coding Agents ` |


A collection of 10 industry-standard Markdown (`.md`) blueprints designed to act as an "onboarding manual" for AI coding agents and LLM assistants. By placing these files in your project root, you dramatically reduce AI hallucinations, prevent scope creep, and save thousands of context window tokens.

## 🚀 Why Use This Framework?
When working with AI tools like **Cursor, Claude Code, Aider, Windsurf, or GitHub Copilot**, LLMs often suffer from "context rotting" or forget project boundaries over long chat sessions. 

This framework solves that by dividing project context into **three specialized pillars**:
1. **Instructions & Rules:** Tell the AI *how* to behave and execute commands.
2. **Project Shaping:** Keep the AI aligned with your roadmap and active sprints.
3. **Architecture & Governance:** Stop the AI from rewriting your tech stack or breaking code style.

---

## 📂 What's Inside the Box?

### 🛠️ Pillar 1: Core AI Instructions & Rules
*   [`AGENTS.md`](./templates/AGENTS.md) - **The Universal AI Directive:** The platform-agnostic standard defining project context, build commands, and safety guardrails.
*   [`CLAUDE.md` / `GEMINI.md`](./templates/CLAUDE.md) - **Tool-Specific Overrides:** Rules optimized for terminal-based CLI agents to dictate how diffs and outputs are formatted.
*   [`SKILL.md`](./templates/SKILL.md) - **Deterministic Runbooks:** Step-by-step workflow documentation that tells the AI exactly how to execute routines safely.

### 📋 Pillar 2: Project Shaping & Management
*   [`PLAN.md`](./templates/PLAN.md) - **The Macro Blueprint:** Sets the high-level vision, core milestones, and strictly defines what is *out of scope*.
*   [`BACKLOG.md`](./templates/BACKLOG.md) - **The Task Queue:** A clean *Now/Next/Later* pipeline that the AI can read to pull its next assignment autonomously.
*   [`SESSIONS.md`](./templates/SESSIONS.md) - **Active Short-Term Memory:** Tracks daily progress and ongoing bugs so the AI can resume work instantly in a fresh chat window.

### 📐 Pillar 3: Architecture & Governance
*   [`DESIGN.md`](./templates/DESIGN.md) - **Visual Tokens & UI Constraints:** Establishes color palettes, layout constraints, and component rules.
*   [`DECISIONS.md`](./templates/DECISIONS.md) - **Architectural Decision Records (ADRs):** Documents *why* technical choices were made so the AI doesn't try to refactor your stack arbitrarily.
*   [`VOICE.md` / `BRAND.md`](./templates/VOICE.md) - **Copy Writing Guidelines:** Ensures AI-generated user copy, errors, and notifications remain on-brand.
*   [`CONTRACT.md`](./templates/CONTRACT.md) - **Environment & API Mocks:** Holds system versions and secure `.env` structures without exposing real secrets.

---

## 🛠️ How to Use

1. **Fork or Clone** this repository.
2. Copy the `templates/` folder into the root directory of your software project.
3. Fill out the `[...]` placeholders inside each file to match your project's technology stack.
4. Point your AI agent to these files (e.g., in Cursor, use `@AGENTS.md` or let autonomous agents discover them natively).

## 💡 Best Practices for Token Optimization
*   **Keep it Lean:** Keep root-level files under 300 lines. If a rule file gets too large, move extended context to a subfolder like `/docs/context/`.
*   **Continuous Audits:** Update your `SESSIONS.md` and `BACKLOG.md` at the end of every programming session to keep the AI's memory fresh.

## 🤝 Contributing
Contributions are welcome! If you have a template optimization or a framework-specific variation (e.g., an `AGENTS.md` optimized purely for Next.js or Python FastAPI), feel free to open a Pull Request.

## 📝 License
This project is open-source and available under the [MIT License](LICENSE).
