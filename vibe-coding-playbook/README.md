# The Vibe Coding Playbook

A practical, opinionated guide to getting the best results from AI-assisted development — updated for March 2026.

![Context Window Budget](./diagrams/01-context-window.svg)

## What's in this repo

| File | What it is |
|---|---|
| [**vibe-coding-playbook.md**](./vibe-coding-playbook.md) | The full guide — 14 sections from vision to production |
| [**agent-instructions.md**](./agent-instructions.md) | Copy-paste blocks for CLAUDE.md / AGENTS.md / .cursorrules |
| [**vibe-coders-stack.md**](./vibe-coders-stack.md) | Companion reference guide for AI-friendly MVP stack and tooling choices |
| [**diagrams/**](./diagrams/) | 5 SVG diagrams embedded in the playbook |

## Who this is for

You use AI coding tools (Claude Code, Cursor, Codex, Copilot, etc.) and want to go beyond basic prompting. You might be building web apps, iOS, Android, or anything else — the playbook is platform-agnostic with optional platform-specific sections you can bolt on.

## The 5 diagrams at a glance

### 1. Context Window Budget
What competes for space inside your agent's brain — and why "less is more" for tools and instructions.

![Context Window](./diagrams/01-context-window.svg)

### 2. Right Altitude Spectrum
How specific your agent instructions should be — the sweet spot between brittle logic and vague hand-waving.

![Right Altitude](./diagrams/02-right-altitude.svg)

### 3. Project File Flow
What gets auto-loaded vs. read on demand vs. written by the agent as it works.

![File Flow](./diagrams/03-file-flow.svg)

### 4. Subagent Architecture
How the main agent delegates to specialists — each with their own model, tools, and isolated context.

![Subagents](./diagrams/04-subagent-architecture.svg)

### 5. Core Development Loop
The Research → Plan → Execute → Review → Ship cycle with feedback loops.

![Dev Loop](./diagrams/05-dev-loop.svg)

## Quick start

**Just want to read the guide?** Open [vibe-coding-playbook.md](./vibe-coding-playbook.md).

**Want to set up your project for AI coding?** Copy the relevant blocks from [agent-instructions.md](./agent-instructions.md) into your project's `AGENTS.md` or `CLAUDE.md`.

**Want the cross-tool setup?** Create `AGENTS.md` as your source of truth, then symlink:

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md .cursorrules
```

## What the playbook covers

1. **Define Your Vision** — foundational decisions before touching code
2. **Context Engineering** — the most important skill for AI-assisted dev (2026)
3. **Plan Your UI/UX First** — design systems, reusable components
4. **Project Setup** — CLAUDE.md / AGENTS.md standard, file structure
5. **Subagents, Skills, and the Agent Ecosystem** — the 2026 toolchain
6. **Prompting and Working with Agents** — spec-driven dev, plan mode, parallel agents
7. **Development Workflow** — Research → Plan → Execute → Review → Ship
8. **Git Workflow** — atomic commits, agent-friendly patterns
9. **Security Best Practices** — the 7 vulnerabilities to always check
10. **Debugging** — systematic approaches when the AI goes in circles
11. **Automation and CLI-First Tools** — one-liner agent instructions
12. **MCP Servers — Less is More** — why restraint beats tool proliferation
13. **Maintaining Your Skills** — don't build on code you don't understand
14. **Advanced Concepts** — inference optimization, routing, production reliability

## Key sources

This playbook synthesizes insights from:

- [Anthropic: Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Manus: Context Engineering Lessons](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
- [Peter Steinberger: Essential Reading for Agentic Engineers](https://steipete.me/posts/2025/essential-reading-august-2025)
- [Claude Code Best Practices (Official)](https://code.claude.com/docs/en/best-practices)
- [HumanLayer: Writing a Good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [Thoughtworks: Context Engineering for Coding Agents](https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html)

Full resource list in the [playbook](./vibe-coding-playbook.md#resources).

## Contributing

Found something outdated or have a pattern that works? Open a PR. The AI coding landscape moves fast — this guide should too.

## License

MIT — use it however you want.
