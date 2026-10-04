> This is a manual context file: open it on purpose. It is not always loaded.

## Development phase

Every area is **shaping** or **settled**: shaping means the design is still being decided and the
developer's judgment is the success criterion, settled means the shape is agreed and the job is to
keep it that way.

- `scripts/doctor/**`, `scripts/tools/**`, `scripts/utils/**` — settled
- `.agents/**`, `docs/**`, and everything else — shaping

**In a shaping area**, spend the budget on something the developer can react to. Skip tests,
guard-test proofs, the pre-work baseline, and doc updates; say in one line what you did not verify.
Use `npm run doctor '--' --fast`. The security baseline below is the only floor. **In a settled
area**, every obligation in the skills applies as written.

**Promotion is deliberate.** Moving an area to settled means that same change adds the tests, the
guard test per bug fixed while shaping, the doc updates, and the `docs/KNOWN_ERRORS.md` entries
that shaping deferred, and deletes the scratch files. Shaping defers those obligations; it does
not abolish them.

**Then hardening, once.** When taste and features are locked and the product is about to be
deployed, run `npm run preflight` and work the hardening pass in the `secure-coding` skill. It
composes verify, the dependency audit, registry signatures, a secret scan, and a strict doctor
run, then names the controls no repository check can see. Entering it is a decision: nothing is
hardened while its shape is still moving.
