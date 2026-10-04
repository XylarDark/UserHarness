> This is a manual context file: open it on purpose. It is not always loaded.

## Applying this template to another project

Copy this repo into the host as `.devenv/` only when the host wants the Node doctor. For game and
engine repositories, copy just the agent and docs layer: `AGENTS.md`, `.agents/skills/` (core),
optionally `.agents/skills-extras/`, the glob-scoped `.cursor/rules/`, `docs/DOCS_LAYOUT.md`,
`docs/KNOWN_ERRORS.md`, `docs/operational/automation-gaps.md`, and `docs/human-use/`.
See `.agents/README.md`.

Host projects write their **own** `AGENTS.md`. The copy in this repo describes this repo.

Skills travel verbatim, so any section of a skill that describes _this_ repository is marked with
a **Localize on copy** callout and must be rewritten by the host. `tests/unit/skill-portability.test.js`
fails if a skill names a repo-local script before that callout, and the integration step reports
which copied skills carry one. Do not add a repo-local command to a skill's `description`: it is
read without opening the file, so it cannot carry a warning.

- **Unity:** keep `.cursor/rules/23-unity-csharp.mdc` and pin the editor version from
  `ProjectSettings/ProjectVersion.txt`. See `docs/templates/unity/README.md`.
- **Unreal:** keep `.cursor/rules/21-unreal-engine.mdc` and `22-unreal-editor-ui.mdc`. See
  `docs/templates/unreal/README.md`.
