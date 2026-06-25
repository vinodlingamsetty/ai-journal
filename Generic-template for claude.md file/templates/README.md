# templates/

Scaffold files for the two-layer Claude instruction system. Copy this folder into any new project to get started.

---

## How It Works

| File | Who writes it | When |
|------|--------------|------|
| `CLAUDE_BASE.md` | You (once, forever) | Before any project |
| `PROJECT_BRIEF.md` | You | At project start |
| `CLAUDE.md` | Claude (generated) | At project start, from the brief |
| `DECISIONS.md` | Claude (maintained) | When non-obvious choices are made |
| `ARCHITECTURE.md` | Claude (maintained) | When system structure changes |
| `TECH_DEBT.md` | Claude (maintained) | When shortcuts are taken |
| `ONBOARDING.md` | Claude (maintained) | When setup steps change |

---

## Starting a New Project

1. Copy `CLAUDE_BASE.md` and this `templates/` folder into your project root
2. Fill in `PROJECT_BRIEF.md`
3. Start a Claude session — it will generate `CLAUDE.md` from the brief and scaffold any missing files automatically

---

## File Roles

**`PROJECT_BRIEF.md`** — The only file you write per project. Describes what the project is, the tech stack, constraints, and decisions already made. Claude reads this once and uses it to generate `CLAUDE.md`.

**`CLAUDE.md`** — The live project instruction file. Generated at project start, updated by Claude as the project evolves. Never hand-edit — Claude owns this file.

**`DECISIONS.md`** — Append-only log of non-obvious choices. Answers "why did we do it this way?" when you return after a long break.

**`ARCHITECTURE.md`** — Current state of the system. Overwritten when structure changes. Answers "how does this thing work?" at a glance.

**`TECH_DEBT.md`** — Append-only log of known shortcuts and deferred fixes. Nothing is deleted — resolved items are marked resolved with a date.

**`ONBOARDING.md`** — How to get the project running from scratch. Overwritten when setup steps change.

---

## Rules

- `CLAUDE_BASE.md` takes precedence over `CLAUDE.md` in all conflicts — base rules cannot be overridden per project
- Append-only files (`DECISIONS.md`, `TECH_DEBT.md`) are never overwritten — only added to
- Overwrite files (`ARCHITECTURE.md`, `ONBOARDING.md`) always reflect current state, not history
- Claude updates these files as a side effect of normal work, not as a separate documentation step
