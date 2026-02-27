# Copilot Instructions for OpenClaw

## Purpose

This document provides comprehensive guidance for GitHub Copilot and AI coding agents working on this repository. It follows [GitHub's best practices for Copilot coding agents](https://gh.io/copilot-coding-agent-tips).

## Quick Reference

- **Language**: TypeScript (ESM, strict mode)
- **Runtime**: Node 22+ (Bun also supported for dev/scripts)
- **Package Manager**: pnpm (keep `pnpm-lock.yaml` in sync)
- **CLI Framework**: Commander + `@clack/prompts`
- **Build**: tsdown (outputs to `dist/`)
- **Lint/Format**: Oxlint + Oxfmt (`pnpm check`)
- **Tests**: Vitest with V8 coverage
- **Channels**: Telegram, Discord, Slack, Signal, iMessage, WhatsApp Web, and extension channels (MS Teams, Matrix, Zalo, etc.)

## Repository Overview

**OpenClaw** is a unified AI messaging gateway that routes messages between messaging platforms (Telegram, Discord, Slack, Signal, iMessage, WhatsApp, and more) and AI agents. It provides:

- A CLI for managing channels, agents, and gateway operations
- Plugin/extension architecture for adding new messaging channels
- AI agent routing with support for multiple AI providers (OpenAI, Anthropic, Google, etc.)
- Real-time message routing and relay between channels
- MCP (Model Context Protocol) integration for enhanced agent capabilities
- Cross-platform support: macOS app, iOS/Android mobile clients, web UI

## Tech Stack

### Core CLI & Gateway
- **Language**: TypeScript (strict ESM)
- **Runtime**: Node 22+ / Bun (dev/scripts)
- **Package Manager**: pnpm
- **CLI**: Commander + `@clack/prompts`
- **Build**: tsdown → `dist/`
- **Lint**: Oxlint (`pnpm check`)
- **Format**: Oxfmt (`pnpm format`)
- **Tests**: Vitest + V8 coverage (`pnpm test`)

### Messaging Channels (Core)
- `src/telegram/` — Telegram bot
- `src/discord/` — Discord bot
- `src/slack/` — Slack integration
- `src/signal/` — Signal messenger
- `src/imessage/` — iMessage (macOS)
- `src/web/` — WhatsApp Web
- `src/channels/` — Channel base types & shared logic
- `src/routing/` — Message routing engine

### Extension Channels (Plugins)
Under `extensions/`:
- `msteams/` — Microsoft Teams
- `matrix/` — Matrix protocol
- `zalo/` — Zalo (Vietnam)
- `zalouser/` — Zalo user account
- `voice-call/` — Voice call integration

### Apps
- `apps/macos/` — macOS menubar app (Swift/SwiftUI)
- `apps/ios/` — iOS app (Swift/SwiftUI)
- `apps/android/` — Android app (Kotlin)

## Project Structure

```
.
├── .github/
│   ├── agents/              # Custom AI agent definitions (13 specialized agents)
│   ├── copilot/             # Copilot MCP configuration (mcp.json)
│   ├── instructions/        # Scoped Copilot instructions
│   └── workflows/           # CI/CD workflows
├── src/                     # Core CLI & gateway source
│   ├── cli/                 # CLI option wiring
│   ├── commands/            # CLI commands
│   ├── channels/            # Channel base types & interfaces
│   ├── routing/             # Message routing logic
│   ├── infra/               # Shared infrastructure utilities
│   ├── terminal/            # Terminal output utilities (tables, themes, progress)
│   ├── media/               # Media pipeline
│   ├── provider-web.ts      # Web provider
│   └── [channel dirs]/      # telegram, discord, slack, signal, imessage, web
├── extensions/              # Plugin/extension channels (workspace packages)
├── apps/                    # Mobile & desktop apps
├── docs/                    # Documentation (Mintlify)
├── scripts/                 # Build, release, and utility scripts
├── dist/                    # Built output (generated)
├── package.json             # Root package manifest
├── pnpm-lock.yaml           # Lockfile (always keep in sync)
└── README.md                # Project documentation
```

## Custom AI Agents

This repository includes **13 specialized agents** for different development tasks:

1. **jules** ⭐ — Code quality, collaboration, refactoring
2. **rapid-implementer** — Fast, autonomous end-to-end code implementation
3. **architect** — System architecture and design decisions
4. **debug-detective** — Advanced debugging and root cause analysis
5. **deep-research** — Comprehensive research and analysis
6. **full-stack-developer** — Complete feature development (CLI + apps)
7. **repo-optimizer** — Repository setup and tooling improvements
8. **testing-stability-expert** — Testing and quality validation
9. **performance-optimizer** — Performance profiling and optimization
10. **code-reviewer** — Security and quality code reviews
11. **docs-master** — Documentation creation and verification
12. **api-developer** — API design and implementation
13. **devops-infrastructure** — Docker, CI/CD, deployment

**Usage**: `@agent:rapid-implementer Implement Slack message threading support`

## MCP Servers Available

### Core Development (Always Active)
- `filesystem` — File operations and batch read/write
- `git` — Version control operations
- `github` — GitHub integration (issues, PRs, code search)
- `memory` — Context persistence across sessions
- `sequential-thinking` — Enhanced multi-step reasoning

### Web & Automation
- `fetch` — HTTP requests to external APIs and docs

### Infrastructure & Utilities
- `docker` — Container management
- `time` — Time and timezone operations

## Development Guidelines

### Code Style & Conventions

#### TypeScript
- **Strict ESM**: All files use `import`/`export` with `.js` extensions
- **No `any`**: Avoid `any`; fix root causes instead of silencing type errors
- **Async/Await**: Use async/await for all I/O operations
- **Error Handling**: Use typed errors with descriptive messages
- **Imports**: Use `.js` extension for cross-package ESM imports
- **Types**: Use `import type { X }` for type-only imports

**CLI Command Pattern:**
```typescript
import { Command } from 'commander';
import { createDefaultDeps } from '../infra/deps.js';

export function registerMyCommand(program: Command): void {
  program
    .command('my-command')
    .description('What the command does')
    .option('--flag', 'Flag description')
    .action(async (options) => {
      const deps = createDefaultDeps();
      // implementation
    });
}
```

**Channel Handler Pattern:**
```typescript
import type { Channel, Message } from '../channels/types.js';

export class MyChannel implements Channel {
  async send(message: Message): Promise<void> {
    // implementation
  }

  async receive(): Promise<Message> {
    // implementation
  }
}
```

**Terminal Output Pattern:**
```typescript
import { renderTable } from '../terminal/table.js';
import { theme } from '../terminal/theme.js';
import { spinner } from '../cli/progress.js';

// Use theme for colors
console.log(theme.success('Operation complete'));

// Use renderTable for tabular data
renderTable(rows, columns);

// Use spinner for long-running ops
const s = spinner();
s.start('Processing...');
// ...
s.stop('Done');
```

### Naming Conventions
- Files: `kebab-case.ts`
- Classes: `PascalCase`
- Functions/variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE` or `camelCase` for module-level consts
- Test files: `*.test.ts` (colocated next to source)

### Testing Patterns

```typescript
// Vitest test pattern
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { MyService } from './my-service.js';

describe('MyService', () => {
  let service: MyService;

  beforeEach(() => {
    service = new MyService();
  });

  it('should do something', async () => {
    const result = await service.doSomething();
    expect(result).toBeDefined();
  });
});
```

### Key Utilities (Always Use These)

- **Time formatting**: `src/infra/format-time` — never create local `formatAge`/`formatDuration`
- **Tables**: `src/terminal/table.ts` (`renderTable`) — for tabular CLI output
- **Colors/themes**: `src/terminal/theme.ts` (`theme.success`, `theme.muted`, etc.)
- **Progress**: `src/cli/progress.ts` — spinners and progress bars
- **Deps injection**: `createDefaultDeps` in `src/infra/` — for CLI commands

## Plugin/Extension System

Extensions live under `extensions/*/` as separate workspace packages.

### Extension Rules
- Plugin-only deps stay in the extension's `package.json`
- Runtime deps must be in `dependencies` (not `devDependencies`)
- Don't use `workspace:*` in `dependencies` (breaks npm install)
- Put `openclaw` in `devDependencies` or `peerDependencies`
- Runtime resolves `openclaw/plugin-sdk` via jiti alias

### Adding a New Channel
When adding a new channel/extension:
1. Create `extensions/<name>/` workspace package
2. Implement the `Channel` interface from `openclaw/plugin-sdk`
3. Update `.github/labeler.yml` and add matching GitHub labels
4. Add documentation in `docs/channels/`
5. Update all UI surfaces (macOS app, web UI, mobile if applicable)
6. Add onboarding flow and status/config forms

## CI/CD

### GitHub Actions Workflows

**CI** (`.github/workflows/ci.yml`):
- Triggered on: Push to main, Pull Requests
- Steps: Install deps → Type-check → Lint → Test → Build

**Install Smoke** (`.github/workflows/install-smoke.yml`):
- Tests the install script end-to-end

**Running Locally:**
```bash
# Install dependencies
pnpm install

# Type-check
pnpm tsgo

# Lint and format check
pnpm check

# Run tests
pnpm test

# Build
pnpm build
```

## Docs (Mintlify)

- Hosted at `https://docs.openclaw.ai`
- Internal doc links: root-relative, no `.md`/`.mdx` extension
- Headings: avoid em dashes and apostrophes (breaks Mintlify anchor links)
- Content: use generic placeholders (no personal device names/hostnames)
- `docs/zh-CN/` is auto-generated — do not edit directly

## Important Notes

1. **Never commit secrets** — use environment variables / `.env`
2. **ESM imports** — always use `.js` extension for local imports
3. **No `any` types** — fix root causes, don't suppress errors
4. **No prototype mutation** — use explicit class inheritance/composition
5. **pnpm only** — use `pnpm install`, not `npm install`
6. **Version locations** — `package.json`, apps `Info.plist`/`build.gradle.kts`
7. **Extensions** — keep plugin deps in extension's `package.json`, not root
8. **Channels** — when refactoring shared logic, update ALL channels (core + extensions)
9. **Test coverage** — maintain ≥70% lines/branches/functions/statements
10. **Patching deps** — requires explicit approval; use exact versions for patched packages

## Pull Request Guidelines

When creating or reviewing PRs:

1. **Before Submitting:**
   ```bash
   pnpm tsgo        # Type check
   pnpm check       # Lint + format
   pnpm test        # Run tests
   pnpm build       # Verify build
   ```

2. **PR Description Should Include:**
   - Summary of changes
   - Related issue numbers
   - Testing performed
   - Breaking changes (if any)
   - Screenshots for UI changes

3. **Code Review Checklist:**
   - [ ] Code follows style guidelines (TypeScript strict ESM)
   - [ ] Tests pass locally (`pnpm test`)
   - [ ] New tests added for new features
   - [ ] Documentation updated (`docs/`)
   - [ ] No secrets in code
   - [ ] Error handling is appropriate
   - [ ] All affected channels updated (not just one)
   - [ ] Extension deps in correct `package.json`

## Getting Help

- **Agent Docs**: `.github/agents/README.md`
- **MCP Config**: `.github/copilot/mcp.json`
- **Workflow Guides**: `.github/agents/CODING_WORKFLOW.md`
- **Architecture**: `docs/`
- **Codebase Patterns**: `.github/instructions/copilot.instructions.md`

---

**When in doubt**: Use the specialized agents! They have deep knowledge of their domains.

**For channel-specific tasks**: The `api-developer` knows messaging protocol patterns, `debug-detective` can trace routing issues, and `full-stack-developer` can implement end-to-end messaging features.
