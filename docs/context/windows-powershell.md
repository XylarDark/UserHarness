> This is a manual context file: open it on purpose. It is not always loaded.

## Windows and PowerShell

This repo is developed on Windows and must work on macOS and Linux.

- Chain commands with `;`, never `&&`.
- Build paths with `path.join`; never hardcode separators.
- Check a path exists before navigating to it.
- Keep commit messages ASCII. Non-ASCII text elsewhere must be valid UTF-8: this repo has twice
  had emoji double-encoded into mojibake, once breaking the linter and once garbling every
  generated plan. `npm run check:encoding` detects it and `fix-mojibake.js --write` repairs it.
