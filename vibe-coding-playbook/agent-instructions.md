# Agent Instructions (Copy-Paste Ready) — March 2026

Drop one of these blocks into your `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, or equivalent agent config file.
Pick the base block, then append any platform-specific sections that apply.

**Important:** Keep your total agent file under 300 lines. Every line competes for attention with actual work. Focus on what the agent would get wrong without the file.

---

## Base Instructions (Use for Any Project)

```markdown
# Project Agent Instructions

## Project Context
<!-- ONE LINE describing your project. Example: -->
<!-- Next.js e-commerce app with Stripe integration and Postgres on Supabase. -->

## Commands
- Build: `<your build command>`
- Test: `<your test command>`
- Lint: `<your lint command>`
- Dev: `<your dev command>`

## Architecture
<!-- 3-5 lines on key boundaries: what's server vs client, data flow, key patterns -->

## Core Principles
- Do ONLY what is explicitly asked. Do not add, remove, or modify anything not requested.
- Ask for clarification rather than guessing when requirements are ambiguous.
- Break large changes into small, verifiable steps.
- Prefer composition over inheritance. Keep functions small and focused.
- Reference existing components when building new ones to maintain consistency.
- Add meaningful error handling — generic messages for users, detailed logs for devs.
- Never hardcode secrets, API keys, or credentials. Use environment variables.

## Context Management
- For complex multi-step tasks, create and maintain a todo.md to track progress.
- Write intermediate results, research, and plans to files in docs/ rather than keeping them in context.
- When asked to review code, use a fresh approach — don't bias toward code you just wrote.
- Add code comments on tricky parts to help both humans and future agent sessions.

## Git — Atomic Commits
Keep commits atomic: commit only the files you touched and list each path explicitly.
- For tracked files: `git commit -m "<scoped message>" -- path/to/file1 path/to/file2`
- For new files: `git restore --staged :/ && git add "path/to/file1" "path/to/file2" && git commit -m "<scoped message>" -- path/to/file1 path/to/file2`

## Testing
- Write tests for any significant feature or change — in the same context as the code.
- Prefer testing behavior over implementation details.

## Documentation
- Write docs to `docs/*.md` — pick descriptive filenames.
- Update relevant docs when changing a feature or subsystem.

## Debugging Protocol
- When an error persists after 2 attempts, stop fixing blindly:
  1. List the components involved and top suspects.
  2. Add targeted logging at suspect locations.
  3. Report findings before attempting another fix.

## Security Defaults
- Validate and sanitize all input on the server side.
- Never trust client-side data for authorization decisions.
- Check resource ownership on every mutating operation.
- Keep `.env` in `.gitignore`. Never commit secrets.
- Use HTTPS everywhere. Rate limit public API endpoints.

## What NOT to Do
- Do not create large monolithic changes. Break them into phases.
- Do not remove or refactor code unrelated to the current task.
- Do not introduce new dependencies without stating why.
- Do not skip error handling or leave TODO placeholders.
- Do not duplicate logic — check for existing utilities first.
```

---

## Web Projects (Next.js / React / TypeScript)

Append this to the base block for web projects:

```markdown
## Tech Stack
- Framework: Next.js (App Router)
- Styling: Tailwind CSS
- Database: Supabase (with RLS enabled)
- Auth: Supabase Auth
- Hosting: Vercel
- Language: TypeScript (strict mode)

## Web-Specific Rules
- All components in `components/` with PascalCase filenames.
- Shared UI primitives (Button, Modal, Loader) in `components/ui/`.
- Server Components by default. Use `"use client"` only when needed.
- API routes validate input with zod or similar before processing.
- All database queries go through server-side code.
- Use Supabase RLS policies. Never bypass with service role key in client code.

## CLI Tools
- Hosting/logs: `vercel` cli
- Database: `psql` (load env from `.env.local`)
- Git: `gh` cli for PRs and issues

## TODO: Web Platform Gotchas
<!-- Expected additions: Common AI mistakes and anti-patterns for web development
- Next.js App Router vs Pages Router: migration patterns, when AI suggests wrong patterns
- Server Components vs Client Components: debugging hydration mismatches from AI code  
- Tailwind class conflicts: when AI generates redundant or conflicting utility classes
- TypeScript strict mode: handling AI-generated code that bypasses type safety
- API route patterns: preventing AI from creating REST endpoints that bypass middleware
- Database query optimization: catching AI-generated N+1 queries and inefficient joins
- Environment variable handling: preventing AI from hardcoding values or wrong env files
-->
```

---

## iOS Projects (Swift / SwiftUI)

```markdown
## Tech Stack
- Language: Swift 5.9+
- UI: SwiftUI
- Architecture: MVVM
- Minimum target: iOS 17

## iOS-Specific Rules
- Views in `Views/`, ViewModels in `ViewModels/`, Models in `Models/`, Services in `Services/`.
- Keep Views declarative — logic belongs in ViewModels.
- Use `@Observable` (Observation framework) over `ObservableObject` for new code.
- Prefer async/await over Combine for new asynchronous code.
- Handle all error states in the UI — no silent failures.
- Use SwiftUI previews for every view with mock data.
- Localize all user-facing strings from the start.

## TODO: iOS Platform Gotchas  
<!-- Expected additions: Common AI mistakes specific to iOS/SwiftUI development
- SwiftUI Preview failures: AI-generated previews that crash due to missing mock data
- @State vs @StateObject confusion: when AI uses wrong property wrappers for data flow
- Memory management: AI creating retain cycles with closures and @escaping parameters
- Threading issues: AI calling UIKit from background threads or updating UI incorrectly
- App lifecycle: AI missing proper handling of foreground/background transitions
- Navigation patterns: AI creating broken NavigationView/NavigationStack hierarchies
- Async image loading: AI not handling failed loads or implementing proper caching
- Accessibility: AI missing VoiceOver labels and semantic roles
-->
```

---

## Android Projects (Kotlin / Compose)

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
- Handle configuration changes gracefully. Use `rememberSaveable` for UI state.
- Define all strings in `strings.xml` — never hardcode user-facing text.

## TODO: Android Platform Gotchas
<!-- Expected additions: Common AI mistakes specific to Android/Compose development  
- Compose state management: AI creating unnecessary recompositions or state hoisting issues
- Coroutine scope confusion: AI using wrong scopes (GlobalScope vs ViewModel vs lifecycle-aware)
- Memory leaks: AI creating context leaks in ViewModels or background tasks
- Permission handling: AI missing runtime permission checks or using deprecated patterns  
- Fragment lifecycle: AI not properly handling fragment transactions and backstack management
- Room database: AI generating inefficient queries or missing migration strategies
- Dependency injection: AI creating circular dependencies or improper scope management with Hilt
- Material Design: AI not following proper theming patterns or accessibility guidelines
- Background processing: AI using deprecated background execution patterns or missing Doze mode considerations
-->
```

---

## Subagent Templates

Create these as `.claude/agents/<name>.md`:

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
- Any `any` type usage in TypeScript (replace with real types)
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

## Common Mistakes File Template

Create this as `mistakes.md` and reference it in prompts for new features:

```markdown
# Common AI Mistakes — Reference This File

When building new features, avoid these patterns that have caused issues before:

## Code Generation
- [ ] Adding imports for unused modules
- [ ] Removing or rewriting code not related to the current task
- [ ] Using deprecated APIs when current alternatives exist
- [ ] Creating overly complex abstractions for simple operations
- [ ] Ignoring existing utility functions and reimplementing them
- [ ] Generating type `any` instead of proper types

## Styling
- [ ] Inconsistent spacing or sizing vs. existing components
- [ ] Hardcoding colors instead of using theme/design tokens
- [ ] Breaking responsive layout on mobile viewports

## Data & State
- [ ] Missing loading states for async operations
- [ ] Missing error states and error boundaries
- [ ] Not handling empty/null data gracefully
- [ ] Race conditions in state updates

## Security
- [ ] Exposing API keys or secrets in client code
- [ ] Missing server-side validation on form inputs
- [ ] Not checking resource ownership before mutations

(Add your own entries as you discover them)
```

---

## Project Docs Folder Template

Create these files in your `docs/` folder at project start:

```
docs/
├── architecture.md    # System design, data flow, server vs client decisions
├── design.md          # Design system, colors, typography, component library
├── features.md        # Feature specs — what each feature does and how
├── requirements.md    # Business and technical requirements
└── decisions.md       # Architectural decisions and why they were made
```

---

## Cross-Tool Compatibility

To use the same agent file across Claude Code, Cursor, Codex, and other tools:

```bash
# Create AGENTS.md as your source of truth
# Then symlink for tool-specific names
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md .cursorrules
```

AGENTS.md is the emerging cross-tool standard backed by the Linux Foundation. CLAUDE.md and .cursorrules are tool-specific but read the same content.

---

## Quick Reference: Claude Code Commands

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
