# 🤖 AI Context Framework Templates


| 📝 License | 🤝 Contributions | 🤖 Target |
| :--- | :--- | :--- |
| ` MIT ` | ` PRs Welcome ` | ` AI Coding Agents ` |


The public site is [mhadikz.github.io/agent-context-framework](https://mhadikz.github.io/agent-context-framework/).

A collection of industry-standard Markdown (`.md`) blueprints designed to act as an "onboarding manual" for AI coding agents and LLM assistants. Generic templates cover any stack. Next.js and NestJS folders add architecture and production practices for small, medium, and large teams. Tool entry points stay at the project root. Everything else lives in `docs/context/`, so agents load it when the task needs it.

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
*   [`RUNBOOK.md`](./templates/RUNBOOK.md) - **Operations:** How to deploy, detect a bad release, roll back, and restore data.
*   [`DATA.md`](./templates/DATA.md) - **Data Register:** What personal data is stored, why, who can read it, and how it is deleted.
*   [`GLOSSARY.md`](./templates/GLOSSARY.md) - **Ubiquitous Language:** Business terms the code must use. Medium and enterprise examples include it.
*   [`CONTEXT-MAP.md`](./templates/CONTEXT-MAP.md) - **Bounded Contexts:** Who owns which data. Enterprise examples include it.

### 🧱 Stack and team-size examples

Filled-in copies of the same files, already arranged the way a project should store them. Copy the **contents** of one size folder into the project root.

| Team | Next.js | NestJS |
| --- | --- | --- |
| Small (about 1–8 engineers, one product) | [`examples/nextjs/small`](./examples/nextjs/small/AGENTS.md) | [`examples/nestjs/small`](./examples/nestjs/small/AGENTS.md) |
| Medium (several squads) | [`examples/nextjs/medium`](./examples/nextjs/medium/AGENTS.md) | [`examples/nestjs/medium`](./examples/nestjs/medium/AGENTS.md) |
| Large / enterprise | [`examples/nextjs/enterprise`](./examples/nextjs/enterprise/AGENTS.md) | [`examples/nestjs/enterprise`](./examples/nestjs/enterprise/AGENTS.md) |

The same rule is baked into every size: production foundations are mandatory; Clean Architecture, DDD, CQRS, event sourcing, and microservices are adopted only when the domain or the organization needs them. The full standard, including what each team size must take from it, is [`ENGINEERING.md`](./ENGINEERING.md).

---

## 🛠️ How to Use

1. **Fork or Clone** this repository.
2. Copy **one** set. For a blank start, copy `templates/` and place the files yourself using the tree below. For Next.js or NestJS, copy the contents of one example folder (`examples/nextjs/<size>` or `examples/nestjs/<size>`) into the project root. That folder is already arranged correctly.
3. The target project should look like this:

```text
AGENTS.md          # project root
CLAUDE.md          # project root
GEMINI.md          # project root
docs/context/
  SKILL.md
  PLAN.md
  BACKLOG.md
  SESSIONS.md
  DESIGN.md
  DECISIONS.md
  VOICE.md
  CONTRACT.md
  RUNBOOK.md
  DATA.md
  GLOSSARY.md       # medium and enterprise
  CONTEXT-MAP.md    # enterprise
```

4. Fill out the `[...]` placeholders inside each file to match your project's technology stack.
5. From `AGENTS.md`, point at `docs/context/` so the agent reads those files at the start of a session and again before architecture, API, or copy changes.

Cursor, GitHub Copilot, and Codex load a root `AGENTS.md` on their own. Claude Code loads a root `CLAUDE.md` (or `.claude/CLAUDE.md`). Gemini CLI loads a root `GEMINI.md`. They do not load `PLAN.md`, `DECISIONS.md`, or the other files unless something points at them. A hidden `.cursor/` folder is the wrong place: other tools and reviewers will not see it. Cursor rules in `.cursor/rules` and Cursor skills in `.cursor/skills/<name>/SKILL.md` are separate from this set.

In a monorepo, keep this single set at the repository root. Add a short `AGENTS.md` inside a package only when that package has different commands or boundaries. Do not copy the full set into every app.

## 💡 Best Practices for Token Optimization
*   **Keep the root to three files:** `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md`. Keep each under 300 lines. The rest stays in `docs/context/`.
*   **Continuous Audits:** Update `docs/context/SESSIONS.md` and `docs/context/BACKLOG.md` at the end of every programming session to keep the AI's memory fresh.

## 🤝 Contributing
Contributions are welcome! If you have a template optimization or a framework-specific variation (e.g., an `AGENTS.md` optimized purely for Next.js or Python FastAPI), feel free to open a Pull Request.

## 📝 License
This project is open-source and available under the [MIT License](LICENSE).
