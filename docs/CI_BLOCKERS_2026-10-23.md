# CI and implementation blockers — 2026-10-23

Scope: QMOOSA-1000 Batch 127, subscription-management tests component, canonical ID QMOOSA-0635.

## Evidence
- The prior `.github/workflows/codeql.yml` used invalid action refs `actions/checkout@4` and `github/codeql-action/*@3`. It also printed `100/100 VERIFIED` without executing tests.
- `tests/reality_battery.py` and `tests/reality_check_harness.py` contain malformed/non-Python content. They are **not** acceptable as passing security or regression tests.
- The repository currently has no separately verified production implementation. README/architecture documents do not establish one.

## Remediation in this review branch
- Use valid GitHub Action refs; configure CodeQL for Python instead of JavaScript/TypeScript.
- Add Python compile and pytest steps that fail closed. A failed test is an accurate signal; do not mark it green by removing the test.
- Keep CodeQL in a separate job, with minimum necessary permissions.

## Required before acceptance
1. Restore valid Python test modules from a reviewed source and add tests that exercise real subscription protocol behavior, including unauthorized access, replay/idempotency, double charge, cancellation and renewal edge cases.
2. Implement missing protocol code where required by the approved architecture, without mocks being counted as implementation evidence.
3. Run tests and CodeQL on the PR head and inspect failed jobs; fix any real findings. Keep the PR draft until they pass.
4. Obtain independent security review before SECURITY_APPROVED or DEPLOYMENT_READY is set.

No FIPS validation, production certification, deployment, gas/cycles spend, or security approval is claimed.
