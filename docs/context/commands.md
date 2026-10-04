> This is a manual context file: open it on purpose. It is not always loaded.

## Commands

Run these from the repository root.

| Command                   | Purpose                                                 |
| ------------------------- | ------------------------------------------------------- |
| `npm run doctor`          | Health check: stack detection, gap analysis, scoring    |
| `npm run doctor:fix`      | Health check, then apply automatic fixes                |
| `npm run build`           | Type-check, then compile to `dist/`                     |
| `npm run build:clean`     | Purge `dist/` first, then build                         |
| `npm test`                | Build, then run `tests/**/*.test.js`                    |
| `npm run lint`            | ESLint over the repo                                    |
| `npm run format`          | Prettier write; `format:check` verifies without writing |
| `npm run clean`           | Remove `dist/` and `tsconfig.tsbuildinfo`               |
| `npm run check:doc-links` | Verify every relative markdown link resolves            |
| `npm run check:encoding`  | Detect double-encoded UTF-8 (mojibake)                  |

**Pass script flags after `--`.** `npm run doctor -- --fix` forwards the flag to the doctor;
`npm run doctor --fix` gives it to npm instead, which silently ignores it. This has been a
recurring source of no-op commands in this repo's own docs and CI.

**In PowerShell, quote the separator:** `npm run doctor '--' --fast`. PowerShell strips a bare
`--` before npm sees it, so the unquoted form silently runs without the flag. Check npm's echoed
command line: it must end in the flag you passed. The quoted form is also correct in bash.

Useful doctor flags: `--fix`, `--no-install`, `--preset <framework>`, `--dry-run`, `--json`,
`--strict` (fail on warnings), `--fast` (skip docs, performance, accessibility, Docker,
environment, git hooks, frameworks, Python tooling), `--project-root <path>`.
