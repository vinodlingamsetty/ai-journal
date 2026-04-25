# Agent Instructions — Copy-Paste Template

Drop the base block into your `AGENTS.md` (symlink to `CLAUDE.md` / `.cursorrules` as needed). Append the platform section that applies. Replace every `<placeholder>` with your own values — **the tech stacks shown are examples, not prescriptions.**

**Budget:** Keep your final file under ~300 lines. Every line competes for the agent's attention with the actual task.

---

## Base Instructions (Use for Any Project)

```markdown
# Project Agent Instructions

## How to Read This File
Read this file fully before starting any task. Re-read the relevant
section when the task touches its area. If a rule here conflicts with
the user's instructions in the current session, ask — don't guess.

## Maintenance
If you notice this file is stale, contradicts reality, or is missing
something you needed during this session, propose an update before
ending. Ask before editing.

## Project Context
<!-- ONE line describing this project. Example: -->
<!-- iOS score-tracking app for tabletop games, SwiftUI + SwiftData. -->

## Related Docs (Don't Duplicate Their Content Here)
- Human-facing overview: `README.md`
- Architectural decisions & history: `docs/adr/`
- Active feature specs: `specs/`
- Known AI anti-patterns for this project: `mistakes.md`
- System design & data flow: `docs/architecture.md`
- Domain vocabulary: `docs/glossary.md`
- Release history: `CHANGELOG.md`

## Project Structure
<!-- 2-level tree of the actual repo. Regenerate when structure shifts. -->
<!-- Generate with: tree -L 2 -I 'build|node_modules|DerivedData|.git' -->
```
<paste your tree here>
```

## Commands
- Build: `<your build command>`
- Test: `<your test command>`
- Lint: `<your lint command>`
- Dev: `<your dev command>`

## Architecture
<!-- 3-5 lines on key boundaries: trust boundaries, data flow, key patterns. -->
<!-- For deeper detail, link to docs/architecture.md — don't expand here. -->

## Core Principles
- Do ONLY what is explicitly asked. Do not add, remove, or modify anything not requested.
- Ask for clarification rather than guessing when requirements are ambiguous.
- Break large changes into small, verifiable steps.
- Prefer composition over inheritance. Keep functions small and focused.
- Reference existing components when building new ones to maintain consistency.
- Add meaningful error handling — generic messages for users, detailed logs for devs.
- Never hardcode secrets, API keys, or credentials. Use environment variables.

## Context Management
- For complex multi-step tasks, create and maintain `todo.md` to track progress.
- Write intermediate results, research, and plans to files in `docs/` rather than keeping them in context.
- When asked to review code, use a fresh approach — don't bias toward code you just wrote.
- Add code comments on tricky parts to help both humans and future agent sessions.

## Git — Atomic Commits
Keep commits atomic: commit only the files you touched and list each path explicitly.
- For tracked files: `git commit -m "<scoped message>" -- path/to/file1 path/to/file2`
- For new files: `git restore --staged :/ && git add "path/to/file1" "path/to/file2" && git commit -m "<scoped message>" -- path/to/file1 path/to/file2`
- Use Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`).
- Reference ADR numbers in commits that implement a decision: `refactor(storage): migrate per ADR-0005`.

## Testing
- Write tests for any significant feature or change — in the same context as the code.
- Prefer testing behavior over implementation details.
- Run the full suite before declaring a task done.

## Documentation
- Write docs to `docs/*.md` — pick descriptive filenames.
- Update relevant docs when changing a feature or subsystem.
- Significant decisions get an ADR in `docs/adr/`, not a paragraph in this file.

## Debugging Protocol
When an error persists after 2 attempts, stop fixing blindly:
  1. List the components involved and top suspects.
  2. Add targeted logging at suspect locations.
  3. Report findings before attempting another fix.

## Security Defaults
- Validate untrusted input at every trust boundary (network, IPC, file, user input).
- Never trust client-side data for authorization decisions.
- Check resource ownership on every mutating operation.
- Keep secrets out of the repo. Sensitive files stay in `.gitignore`.
- Apply least privilege to tokens, API keys, and service accounts.

## What NOT to Do
- Do not create large monolithic changes. Break them into phases.
- Do not remove or refactor code unrelated to the current task.
- Do not introduce new dependencies without stating why.
- Do not skip error handling or leave TODO placeholders.
- Do not duplicate logic — check for existing utilities first.
- Do not expand this file with content that belongs in ADRs, specs, or `mistakes.md`.
```

---

## Standard Project Structure (Stack-Agnostic)

Common skeleton that works for any project — web, mobile, CLI, library. Stack-specific files (like `Package.swift`, `package.json`, `build.gradle.kts`) sit alongside it but aren't listed here.

```
project-root/
│
├── README.md
│   <!-- Human-facing entry point. What the project is, who it's for,
│        how to run it locally, how to contribute. First thing a new
│        developer (or you, six months from now) reads. Keep it short;
│        link out to docs/ for depth. -->
│
├── AGENTS.md
│   <!-- This file. Instructions for AI coding agents. Symlink
│        CLAUDE.md and .cursorrules to this so all tools read the same
│        rules. Current state only — no history, no aspirations. -->
│
├── CHANGELOG.md
│   <!-- User-facing record of what changed per release. Use Keep a
│        Changelog format (keepachangelog.com). Sections: Added,
│        Changed, Deprecated, Removed, Fixed, Security. One entry per
│        version, newest at top. Update as part of every release. -->
│
├── mistakes.md
│   <!-- Living log of AI anti-patterns discovered on THIS project.
│        Not universal rules (those go in AGENTS.md) — project-specific
│        gotchas like "AI keeps suggesting X but our codebase does Y."
│        Add entries as you catch them in code review. -->
│
├── todo.md
│   <!-- Scratchpad for the CURRENT multi-step task. Created/cleared
│        per task, not kept permanently. Agent writes plan here,
│        checks off items, dumps intermediate findings. Prevents
│        context bloat from keeping everything in chat. Gitignored
│        OR committed briefly then deleted — your call. -->
│
├── .gitignore
│   <!-- Stack-appropriate ignores: build artifacts, dependency
│        caches, env files, IDE files, OS junk. Never commit secrets,
│        credentials, or generated binaries. Start from a template at
│        gitignore.io for your stack. -->
│
├── .env.example
│   <!-- Template showing every env var the app needs, with dummy
│        values and comments explaining each. Committed to the repo.
│        Real .env (with actual secrets) is gitignored. New developers
│        copy .env.example → .env and fill in real values. -->
│
├── docs/
│   │   <!-- All long-form documentation lives here. AGENTS.md and
│   │        README stay short; docs/ holds the depth. -->
│   │
│   ├── architecture.md
│   │   <!-- System design: components, data flow, trust boundaries,
│   │        key patterns, external dependencies. The "how it fits
│   │        together" document. Include a diagram if useful. Update
│   │        when structure changes meaningfully. -->
│   │
│   ├── glossary.md
│   │   <!-- Domain vocabulary. Terms specific to your project/domain
│   │        with definitions. Example for a score tracker: "Match"
│   │        vs "Game" vs "Round" vs "Turn." Ensures agents (and
│   │        humans) use consistent language. -->
│   │
│   ├── requirements.md
│   │   <!-- What the product needs to do. Business rules, user
│   │        needs, non-functional requirements (performance, privacy,
│   │        accessibility). The "why" behind features. -->
│   │
│   ├── features.md
│   │   <!-- Catalog of shipped features: what each does, where it
│   │        lives in the code, any non-obvious behavior. Distinct
│   │        from specs/ (which is forward-looking). -->
│   │
│   ├── design.md
│   │   <!-- Visual/UX design system: colors, typography, spacing,
│   │        component patterns, iconography. If you have a Figma
│   │        file, link it here and summarize the tokens. -->
│   │
│   ├── prompts/
│   │   │   <!-- Reusable prompt templates for recurring agent tasks.
│   │   │        Paste these into a session to get consistent results
│   │   │        across sessions and team members. -->
│   │   ├── add-feature.md       <!-- "Add a new feature X following our patterns..." -->
│   │   ├── write-tests.md       <!-- "Write tests for file Y, covering..." -->
│   │   ├── refactor.md          <!-- "Refactor Z without changing behavior..." -->
│   │   └── code-review.md       <!-- "Review this PR for our specific concerns..." -->
│   │
│   └── adr/
│       │   <!-- Architectural Decision Records. One file per
│       │        significant decision, numbered sequentially. Captures
│       │        context, decision, consequences. Never edit past ADRs
│       │        — supersede them with new ones. This is your
│       │        project's history and reasoning trail. -->
│       ├── 0001-<decision-slug>.md
│       └── 0002-<decision-slug>.md
│
├── specs/
│   │   <!-- Forward-looking feature specifications. What you're
│   │        ABOUT to build, not what you've built. Each spec captures
│   │        intent, acceptance criteria, constraints. Agent reads
│   │        these before implementing. Delete or archive after the
│   │        feature ships (or move content into docs/features.md). -->
│   ├── 0001-<feature-slug>.md
│   └── 0002-<feature-slug>.md
│
├── scripts/
│   │   <!-- Project automation scripts: setup, build, deploy, data
│   │        migration, dev-environment bootstrap. Keeps agents from
│   │        reinventing commands and ensures everyone runs the same
│   │        steps. Each script should be self-documenting (header
│   │        comment explaining what it does and how to run it). -->
│   ├── setup.sh             <!-- First-time environment setup -->
│   ├── build.sh             <!-- Full build pipeline -->
│   └── deploy.sh            <!-- Deploy to staging/prod -->
│
├── .claude/                     <!-- Claude Code-specific config -->
│   ├── agents/
│   │   │   <!-- Subagent definitions. Specialized agents for narrow
│   │   │        tasks (security review, test writing) with their own
│   │   │        tools, model, and permission modes. Keeps the main
│   │   │        agent focused. -->
│   │   ├── security-reviewer.md
│   │   ├── code-reviewer.md
│   │   └── test-writer.md
│   │
│   └── commands/
│       │   <!-- Slash commands for recurring workflows. Type
│       │        /<command-name> in Claude Code to invoke. Good for
│       │        multi-step routines you run often. -->
│       ├── ship-feature.md
│       └── weekly-cleanup.md
│
└── src/  (or app/, lib/, etc. — names vary by stack)
    <!-- Actual source code. Structure depends on the stack and
         architecture. Document the structure in docs/architecture.md
         and summarize it in AGENTS.md's "Project Structure" section. -->
```

---

## Platform Examples (Reference Only — Do Not Copy Wholesale)

These show how the stack-agnostic structure above gets specialized per platform. Replace versions, libraries, and architecture choices with yours.

### Example: Web (Next.js / React / TypeScript)

```markdown
## Tech Stack
- Framework: Next.js 15 (App Router)
- Language: TypeScript (strict mode)
- Styling: Tailwind CSS
- Database: Postgres (Supabase, with RLS enabled)
- Auth: Supabase Auth
- Hosting: Vercel

## Web-Specific Rules
- All components in `components/` with PascalCase filenames.
- Shared UI primitives (Button, Modal, Loader) in `components/ui/`.
- Server Components by default. Use `"use client"` only when needed.
- API routes validate input with zod (or similar) before processing.
- All database queries go through server-side code.
- Use Supabase RLS policies. Never use the service-role key in client code.
- Never put secrets in `NEXT_PUBLIC_*` env vars.

## Stack-specific files (alongside the standard structure)
- `package.json`, `tsconfig.json`, `.eslintrc`, `.prettierrc`, `next.config.js`

## CLI Tools
- Hosting/logs: `vercel` cli
- Database: `psql` (load env from `.env.local`)
- Git: `gh` cli for PRs and issues
```

### Example: iOS (Swift / SwiftUI)

```markdown
## Tech Stack
- Language: Swift 5.9+
- UI: SwiftUI
- Architecture: MVVM
- Persistence: SwiftData
- Minimum target: iOS 17

## iOS-Specific Rules
- Views in `Views/`, ViewModels in `ViewModels/`, Models in `Models/`, Services in `Services/`.
- Keep Views declarative — logic belongs in ViewModels.
- Use `@Observable` (Observation framework) over `ObservableObject` for new code.
- Prefer async/await over Combine for new asynchronous code.
- Use SwiftUI previews for every view with mock data.
- Localize all user-facing strings from the start (no hardcoded English).
- Store credentials and tokens in Keychain, not UserDefaults.

## Stack-specific files (alongside the standard structure)
- `Package.swift` or `Podfile`, `.swiftlint.yml`, `fastlane/`, `*.xcodeproj`

## CLI Tools
- Build: `xcodebuild -scheme <Scheme> -destination 'platform=iOS Simulator,name=iPhone 15'`
- Test: `xcodebuild test -scheme <Scheme> -destination '...'`
```

### Example: Android (Kotlin / Compose)

```markdown
## Tech Stack
- Language: Kotlin
- UI: Jetpack Compose
- Architecture: MVVM with clean architecture layers
- DI: Hilt
- Minimum API: 26

## Android-Specific Rules
- Follow `data/domain/presentation` layer structure.
- Composables in `ui/`, ViewModels in `viewmodel/`, Repositories in `data/repository/`.
- Keep Composables stateless where possible — hoist state to ViewModels.
- Use Kotlin coroutines and Flow for async work. No callbacks.
- Use lifecycle-aware coroutine scopes (`viewModelScope`, `lifecycleScope`). Avoid `GlobalScope`.
- Define all strings in `strings.xml` — never hardcode user-facing text.
- Store secrets via EncryptedSharedPreferences or the Keystore system.

## Stack-specific files (alongside the standard structure)
- `build.gradle.kts`, `settings.gradle.kts`, `detekt.yml`, `proguard-rules.pro`

## CLI Tools
- Build: `./gradlew assembleDebug`
- Test: `./gradlew test`
```

---

## Subagent Templates

Create these as `.claude/agents/<n>.md`:

### Security Reviewer

```yaml
---
name: security-reviewer
description: Reviews code for security vulnerabilities — injection, auth flaws, exposed secrets
tools: Read, Grep, Glob
model: opus
permissionMode: plan
---
You are a senior security engineer. Review code for:
- Injection vulnerabilities (SQL, XSS, command injection)
- Authentication and authorization flaws
- Secrets or credentials in code
- Insecure data handling
- Missing input validation on server-side endpoints
- IDOR vulnerabilities (missing ownership checks)
Provide specific line references and suggested fixes. Do not modify any files.
```

### Code Reviewer

```yaml
---
name: code-reviewer
description: Reviews code for quality, reuse, and correctness after implementation
tools: Read, Grep, Glob, Bash
model: sonnet
permissionMode: plan
---
Review the code for:
- Functions longer than 30 lines (likely doing too much)
- Logic duplicated more than twice (extract to utility)
- Any loosely-typed constructs that should be strongly typed
- Missing error handling on async operations
- Components with more than 3 props that could be grouped
- Deviation from existing patterns in the codebase
Suggest specific improvements. Do not apply changes.
```

### Test Writer

```yaml
---
name: test-writer
description: Writes tests for new or changed code — invoke after implementation
tools: Read, Grep, Glob, Edit, Bash
model: sonnet
---
Write tests for the code changes in this session. Focus on:
- Behavior, not implementation details
- Edge cases and error paths
- Integration points between modules
Run the test suite after writing to verify tests pass.
```

---

## Cross-Tool Compatibility

`AGENTS.md` is the emerging cross-tool standard. To share one file across Claude Code, Cursor, Codex, etc.:

```bash
# AGENTS.md is the source of truth. Symlink tool-specific names to it:
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md .cursorrules
```

Claude Code also loads `~/.claude/CLAUDE.md` as a user-level file alongside the project file — good place for your personal universal rules that apply across all your projects.

---

## First-Time Setup Note

If you want an agent to scaffold this structure on a fresh repo, add this block to the top of your AGENTS.md temporarily, then **delete it after the first run**:

```markdown
## First-Time Setup (DELETE AFTER RUNNING)
On first read, create these files/folders if they don't exist, leaving
them empty (or with the header comment only — I'll fill them in):
- README.md, CHANGELOG.md, mistakes.md, .env.example, .gitignore
- docs/architecture.md, docs/glossary.md, docs/requirements.md,
  docs/features.md, docs/design.md
- docs/adr/ (empty), specs/ (empty), scripts/ (empty)
- docs/prompts/ with placeholder templates
- .claude/agents/ and .claude/commands/ (empty)
Do NOT create src/ — I'll structure that per my stack.
```

Leaving this block in permanently will cause every future session to re-check and potentially re-create things. Delete it once setup is done.

---

## Quick Reference: Agent CLI Commands

### Claude Code

| Command | What it does |
|---|---|
| `/compact` | Manually compress context (do this at ~50% usage) |
| `/clear` | Reset context for a new task |
| `/context` | See current context window usage |
| `/model` | Switch model (Opus for planning, Sonnet for coding) |
| `/config` | Configure settings (thinking mode, output style) |
| `/usage` | Check plan limits |
| `/plugin` | Browse and install marketplace plugins |
| `/rename` | Rename important sessions for later reference |
| `ultrathink` | Keyword in prompts for high-effort reasoning |
| `!<command>` | Run shell command directly without token evaluation |

### Codex (OpenAI)

| Command / Flag | What it does |
|---|---|
| `codex "<task>"` | One-shot task — runs and exits |
| `codex` | Start interactive session |
| `--approval-mode suggest` | Read-only: suggests changes, you apply |
| `--approval-mode auto-edit` | Edits files, asks before running commands |
| `--approval-mode full-auto` | Fully autonomous — no confirmation prompts |
| `--model <id>` / `-m` | Select model (e.g. `o4-mini`, `o3`) |
| `--quiet` / `-q` | Suppress non-essential output |
| `--project-doc <file>` | Use a custom agent instructions file |
| `--no-project-doc` | Skip AGENTS.md for this run |
| `--full-auto` | Shorthand for `--approval-mode full-auto` |
