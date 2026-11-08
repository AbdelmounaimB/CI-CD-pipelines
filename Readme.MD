# CI/CD Pipeline Template Library

A reference set of standalone GitHub Actions pipelines, one per concept, ordered
from the simplest possible pipeline up to advanced, production-grade patterns
(security gates, progressive delivery, GitOps, chaos, observability).

Each workflow file is runnable on its own and paired with a doc in `docs/`
explaining: what it does, why it's built that way, and the trade-offs.

## Roadmap (simple → advanced)

| # | Workflow | Stage | Concept |
|---|----------|-------|---------|
| 01 | `01-minimal-ci.yml` | Foundation | Smallest possible pipeline: trigger, checkout, run |
| 02 | `02-lint-format.yml` | Code quality | Static formatting/lint gate |
| 03 | `03-pre-commit-ci.yml` | Code quality | Enforce local pre-commit hooks in CI |
| 04 | `04-commit-lint-semver.yml` | Code quality | Conventional commits → automated semver |
| 05 | `05-code-review-gate.yml` | Code quality | CODEOWNERS + required review enforcement |
| 06 | `06-unit-tests-coverage.yml` | Testing | Unit tests + coverage threshold gate |
| 07 | `07-matrix-testing.yml` | Testing | OS/version matrix strategy |
| 08 | `08-integration-tests.yml` | Testing | Service containers (DB/cache) in CI |
| 09 | `09-contract-tests.yml` | Testing | Pact consumer/provider verification |
| 10 | `10-e2e-tests.yml` | Testing | Playwright against an ephemeral preview env |
| 11 | `11-performance-tests-k6.yml` | Testing | Load test as a merge/deploy gate |
| 12 | `12-mutation-testing.yml` | Testing | Mutation score gate (tests test the tests) |

Templates are added incrementally — check each PR/commit for the latest set.

## Design principles applied across every template

1. **Fail fast, fail cheap** — cheapest/fastest checks (lint, secrets) run before
   expensive ones (E2E, performance).
2. **Least privilege** — `permissions:` block scoped per job, never blanket `write-all`.
3. **Pinned actions** — third-party actions pinned to a commit SHA, not a floating tag.
4. **Cacheable & idempotent** — dependency/build caches keyed correctly; re-runs are safe.
5. **Explicit over implicit** — no hidden global env; every gate's pass/fail criteria is visible in the YAML.
6. **Separation of concerns** — one concept per file; composition (reusable workflows) instead of one mega-pipeline.