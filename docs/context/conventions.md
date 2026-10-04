> This is a manual context file: open it on purpose. It is not always loaded.

## Conventions

- **Commits:** Conventional Commits (`feat`, `fix`, `docs`, `refactor`, `perf`, `test`, `chore`,
  `style`, `ci`). Imperative mood, first line under 72 characters, no emoji. Body lines wrap at
  100 characters. Commitlint enforces this in a hook.
- **Branches:** `feat/`, `fix/`, `refactor/`, `perf/`, `docs/`, `test/`, `chore/`.
- **Files:** kebab-case (`user-service.ts`). Name the file after its primary export.
- **Naming:** functions are verbs, types are nouns, booleans read as questions, constants are
  `UPPER_SNAKE_CASE`.
- **Docs:** place new documents per `docs/DOCS_LAYOUT.md`. The docs root is a closed set of entry
  points; topic documents belong in a subdirectory. Run `npm run check:doc-links` after moving
  or renaming anything.
