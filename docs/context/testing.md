> This is a manual context file: open it on purpose. It is not always loaded.

## Testing

These apply to settled areas. While shaping, see **Development phase** above.

- Unit tests finish in under 5 seconds total; integration tests in under 60.
- Every test needs a timeout, must run independently, and must clean up in `afterEach`.
- Use real temporary directories (`fs.mkdtemp`), not `mock-fs` — this repo runs on Windows too.
- Test behavior, not implementation. Feature work uses user-flow (BDD) scenarios; a failing
  test is still the definition of done in settled areas. Classic red-green TDD is for
  critical or low-level units and for regression guards. If you skip tests, say why.
