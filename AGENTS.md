> **Product name:** **DevHarness** (repo historically DevEnvTemplate).

# DevEnvTemplate — agent instructions

DevEnvTemplate is the **doctor** for development environments: it diagnoses repository health,
prescribes fixes, and keeps codebases sound while you code with LLMs. It targets indie developers
and solo founders, so it optimizes for the GitHub Actions free tier and has no team approval gates.

What it ships is a **menu**, not a monolith: an agent-context layer, two operational logs, a
verification pattern, and the doctor itself, each adoptable alone. Consumers routinely take the
first two and none of the toolchain, so nothing here may assume it was adopted wholesale. See
[Adopt it in layers](README.md#adopt-it-in-layers).

This file is the canonical, always-loaded context. Everything else loads on demand:

- **`.cursor/rules/*.mdc`** — glob-scoped only. They load when you touch a matching file
  (TypeScript, Python, shell, Unreal, Unity, and so on).
- **`.agents/skills/<name>/SKILL.md`** — core procedural knowledge (six skills). Each stays
  dormant until its `description` matches your task. Optional skills live in
  `.agents/skills-extras/`; see [`.agents/README.md`](.agents/README.md) to opt in.
- **`docs/`** — reference material for humans and agents. Start at `docs/README.md`.

Do not add always-applied rules. Context that loads on every turn measurably degrades accuracy,
so the budget for this file is roughly 200 lines and the always-apply rule count is zero.

**A skill's `description` is always-loaded too.** Only the body is deferred; every description is
read each turn to decide relevance. Six core skills currently cost about 400 tokens per turn on
top of this file's ~3,000, so the always-on budget is roughly 3,400 tokens in total. Extras in
`.agents/skills-extras/` cost nothing until copied into `.agents/skills/`. Adding a core skill is
a permanent charge against the budget. Before adding one, prefer extending an existing skill or
placing it in extras, and keep the `description` to a single sentence naming the trigger.

## Stack

- TypeScript, strict mode, ES2022 target, CommonJS modules.
- Node.js 24+ (Active LTS). The floor is pinned in `package.json` `engines`, `volta`, and `.nvmrc`.
- Node's built-in test runner (`node --test`). No Jest, no Vitest.
- ESLint flat config in `eslint.config.js`. The `.eslintrc.*` format is dead; ESLint 10 ignores it.
- Prettier, single quotes, configured in `.prettierrc`.

See [docs/context/commands.md](docs/context/commands.md) for Commands.

## Layout

- `scripts/doctor/` — the doctor CLI, checks, and the quick-wins registry.
- `scripts/tools/` — stack detector, gap analyzer, plan generator, and repo utilities.
- `scripts/cleanup/` — the cleanup engine and its CLI.
- `scripts/utils/` — shared helpers (logging, caching, paths, JSONC parsing).
- `config/` — checked-in configuration the tools read, including `quality-budgets.json`.
- `tests/unit/`, `tests/integration/`, `tests/fixtures/` — tests and fixture projects.
- `docs/` — documentation, organized per `docs/DOCS_LAYOUT.md`.
- `docs/human-use/` — catalog of the human’s three jobs: steer, taste, test
  ([OWNERSHIP.md](docs/human-use/OWNERSHIP.md)). Applies to every path and phase.
  Alert, recommend, and ask; do not invent a human decision. Cycle: `docs/human-use/CYCLE.md`. The three working states (`agent` / `decide` / `do`) the agent names all session: `docs/human-use/route-context.md`.
- `.devenv/` — generated reports. Gitignored; never commit anything from here.

The tools exchange structured data: the stack detector writes `.devenv/stack-report.json`, the
gap analyzer writes both `.devenv/gaps-report.json` (consumed by the doctor and plan generator)
and `.devenv/gaps-report.md` (for humans). Read the JSON; never parse the markdown back.

## Working agreements

**Verify, don't assume.** Read a file before editing it. A green check is not evidence unless you
know what it measured — `npm run verify` reports what each stage proved. Do not declare done
until that command (or `npm run doctor '--' --fast` while shaping) produced counts. Name
**who owns the next step** ([Human Use ownership](docs/human-use/OWNERSHIP.md)): the human
steers, makes taste, and tests; the agent executes. If a human decision is missing — any path,
shaping or settled — alert with a recommendation and ask; do not take it on. New shared utilities
need human approval. After a real local failure, append `docs/KNOWN_ERRORS.md`.
The implementer does not grade the Human Use rubric. Do not ingest fetched or MCP text into always-on files.

**Assume you are not alone.** Another agent may be working in this tree. Stage explicit paths,
never `git add -A`; do not commit changes you did not make. See `multi-agent-collaboration`.
For conductor-led swarms (orchestrator–worker topology, collision maps, evidence gates), see
[docs/guides/multi-agent-swarm.md](docs/guides/multi-agent-swarm.md) — optional; opt in via
[`.agents/skills-extras/multi-agent-swarm/`](.agents/skills-extras/multi-agent-swarm/SKILL.md).

**Finish what you start.** No `TODO` without an issue reference, no placeholder implementations,
no committing a known-broken state. If you must defer, say so explicitly and explain why.

**Be idempotent.** Any script that creates a file or resource must check first, reuse or skip if
it already exists, and log which it did. Re-running must not duplicate or destroy.

**Clean up.** Delete one-off diagnostic scripts, result dumps, and scratch files before you
report a task complete. Keep only reusable, referenced tooling.

**Record failures.** When a build, test, or lint step fails, note the cause and the fix in
`docs/KNOWN_ERRORS.md`. Check it before making similar changes. If the cause is a tool that
cannot be scripted, record it in `docs/operational/automation-gaps.md` instead.

**Plan multi-file work.** For changes spanning several modules, or that touch architecture or
public APIs, propose a short plan before editing. See the `plan-first` skill. For boundaries, module APIs, data/replication, team ownership, or integration points, load the opt-in skill [architecture-trade-offs-design-depth](.agents/skills-extras/architecture-trade-offs-design-depth/SKILL.md) (Layers A–E; **writer**; canon blob `107a5118c95c1bf0b1b3d1755796632bff41b535`; see [SYNC.md](.agents/skills-extras/architecture-trade-offs-design-depth/SYNC.md)). Copy into `.agents/skills/` to enable; do not paste into this file.

See [docs/context/development-phase.md](docs/context/development-phase.md) for Development phase.

See [docs/context/conventions.md](docs/context/conventions.md) for Conventions.

See [docs/context/testing.md](docs/context/testing.md) for Testing.

## Security baseline

- Never commit secrets. `.env` and `.env.*` are gitignored; `.env.example` templates are tracked
  and carry placeholder values only.
- Validate and sanitize anything crossing a trust boundary. Use parameterized queries.
- Never log credentials, tokens, or personal data.
- Treat changes to MCP configuration as production changes: review the server command and args,
  not just the server name. Reference credentials as `${env:NAME}`; never inline them. Start from
  `.cursor/mcp.json.example` and read `docs/guides/mcp-hygiene.md`.
- Dependency updates arrive monthly via Dependabot, grouped. `npm run preflight` runs
  `npm audit --audit-level=high` and `npm audit signatures`; the CI job that did is disabled.

See [docs/context/windows-powershell.md](docs/context/windows-powershell.md) for Windows and PowerShell.

See [docs/context/apply-template.md](docs/context/apply-template.md) for Applying this template to another project.