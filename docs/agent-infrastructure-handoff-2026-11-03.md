# Agent Infrastructure Handoff — 2026-11-03 (WIP draft)

> **Status: WIP DRAFT on branch `feat/upstream-agent-infrastructure`. NOT main, NOT production-ready. No merges / no main bypass.**
> Machine-move checkpoint only. Implementation paused for machine move — do NOT mark the original migration complete. Only publication handoff is complete.
> Parent (human/parent agent) owns all Git: fetch/push, final PR links, immutable snapshot SHAs, tests, review.

## 1. What this is

Freeze work and push ALL local changes to origin for continuation on another computer. No further feature implementation in this session. This document + the machine-readable sibling checkpoint JSON are the documentation/checkpoint owned by the default OpenCode agent in this session:

- This file: `.github/docs/agent-infrastructure-handoff-2026-11-03.md`
- Machine-readable: `.github/docs/agent-infrastructure-checkpoint/agent-infrastructure-checkpoint-2026-11-03.json`
- Immutable source commits (PRESERVE, do not rewrite): `.github/docs/agent-infrastructure-checkpoint/git-snapshot.json` — https://github.com/mehomeai/.github/blob/feat/upstream-agent-infrastructure/docs/agent-infrastructure-checkpoint/git-snapshot.json
- Curated contracts + known-open review list: `.github/docs/agent-infrastructure-checkpoint/` (sanitized; no raw logs, shell env, tokens, OpenCode database/history, private licensed config)
- Short per-repo pointers: `docs/SESSION_HANDOFF.md` in `agentic-framework`, `infra-server`, `setup-infra`, `phone-app`, `backend-server` (small navigation note in `web-app` / `feed-algorithm` only where already natural — none added gratuitously here)

Normal OpenCode mandate: **all implementation through `opencode run` with the default model; no model override.** Parent review / tests / Git allowed.

## 2. Repo set and branches (WIP draft)

| Repo | Branch for this work |
|---|---|
| `.github` (this repo) | `feat/upstream-agent-infrastructure` (WIP draft) |
| `agentic-framework` | `feat/upstream-agent-infrastructure` (WIP draft) |
| `infra-server` | `feat/upstream-agent-infrastructure` (WIP draft) |
| `setup-infra` | `feat/upstream-agent-infrastructure` (WIP draft) |
| `phone-app` | `feat/upstream-agent-infrastructure` (WIP draft) |
| `backend-server` | `feat/upstream-agent-infrastructure` (WIP draft) |
| `web-app` | `product/demo-v1-structure` |
| `feed-algorithm` | `main` |

Every repo must be clonable from origin; parent will fetch/push all 8.

### Restore snippet (run from the new machine)

```sh
ORG=mehomeai   # origin org; adjust if your remote differs
ROOT=~/me-restore
mkdir -p "$ROOT" && cd "$ROOT"
for r in agentic-framework infra-server setup-infra phone-app backend-server web-app feed-algorithm; do
  git clone "git@github.com:${ORG}/${r}.git"
done
git clone "git@github.com:${ORG}/.github.git" me-github
# check out WIP draft branches
for r in agentic-framework infra-server setup-infra phone-app backend-server; do
  git -C "$r" checkout feat/upstream-agent-infrastructure
done
git -C me-github checkout feat/upstream-agent-infrastructure
git -C web-app checkout product/demo-v1-structure
git -C feed-algorithm checkout main
git -C agentic-framework status --short --branch
git -C infra-server status --short --branch
```

Relative paths only from here on; do not rely on `/private/tmp` or local session IDs for restart.

## 3. Product direction (locked)

- Expo phone app owns its own UI → home infra on a separate computer (family / multiple sessions); hosted backend is coordination only.
- Private conversations/files execute/store on home. No cloud-memory fallback, no fake production-ready fixture.
- User explicitly allowed licensed SELF-HOSTED CopilotKit Intelligence; **no actual license / provider keys / Kubernetes supplied** in this session.
- Reuse the actual pinned OpenBot full platform as primary; minimal Me identity/UI/installer glue; legacy explicit read/resume.
- Actual OpenBot mobile REST, not fake raw AGUI `/api/copilotkit`.
- Native push / artifact parity incomplete: push stays disabled until a generic privacy relay exists; no complete native backup/restore (optional capability, `--require-backup` strict). Keep operational READY distinct from full backup readiness.

## 4. Upstream pins (exact source graph, not a new homebrew engine)

- OpenBot (CopilotKit/OpenBot): `6bfee780c8e8d05141375ea55ebde33eefb3dbc0`
- OpenMuse: `a4944f4c471d3dc93b8bfc060b5da309b5216ab2`
- OpenDuet: `f52288fff5ccdbd3de428d573d9f026fce5ff480`
- OpenDots: `d4214aff10f066911f3253cb3cb870764cd51a9f`
- MIT notices / manifest tracked in-repo. Candidate update workflow: read-only tests before any review PR; no automerge / deploy.

## 5. Published PRs (final publication handoff complete; migration still paused)

- Runtime: https://github.com/mehomeai/agentic-framework/pull/2 — latest published `3dadb75dd3cec4854466b0b15f2680904fe69db7`
- Phone: https://github.com/mehomeai/phone-app/pull/7 — `abc3a55a6d8e9cab2a8121f882d12f2c50a0ee25`
- infra-server: https://github.com/mehomeai/infra-server/pull/3
- setup-infra: https://github.com/mehomeai/setup-infra/pull/2
- backend-server: https://github.com/mehomeai/backend-server/pull/2
- Exact immutable source commits: https://github.com/mehomeai/.github/blob/feat/upstream-agent-infrastructure/docs/agent-infrastructure-checkpoint/git-snapshot.json (preserve; do not rewrite)
- No merges / main bypass. Final post-push CI pending / not awaited.

## 6. Completed / validated reports (as reported before final parent checks)

These precede final combined parent acceptance and are **not** actual licensed E2E:

- Phone: 147 unit + 2 real integration + strict official Expo/RN TS.
- Native transport: 25/25 PASS actual phone/home against explicit upstream REST FIXTURE (not licensed full platform). Correct cmd: `node --experimental-transform-types --disable-warning=ExperimentalWarning scripts/acceptance-native.mjs`. First attempt without transform flag failed; corrected cmd passed.
- Isolated auth patch: 15 cases.
- Infra worker: home 181 pass / 0 fail / 1 pre-existing R2 sibling skip (of 182) via `npm run test:home` — parent freshly verified; upstream 78; Python 17.
- Runtime Linux CI success at latest published `3dadb75`: runs `41735540383` (image), `41735539610` (CI), `41735539612` (dependency). Earlier baseline 142 pass / 1 skip.
- Runtime new uncommitted local Ollama fix: 17 targeted pass — actual SDK requires non-secret `OPENAI_API_KEY=ollama`; verification previously masked it.
- Installer worker was interrupted on user freeze while doing matching effective-key tests; **partial code MUST be checkpointed and reviewed, do not claim finished.**
- CRITICAL ACTUAL RESULT — setup-infra `python3 -m unittest discover -s scripts -p "test_me*.py"`: ran 166 tests, 268.950s, FAILED 3. Failures (highest-priority resume blockers, source deliberately preserved unfinished, do NOT fix now): `test_imported_dependency_tamper_fails_closure` (DoctorIntegrityCases), `test_generic_key_satisfies_native_check` (NativeKeyCases), `test_operational_ready_reachable_without_backup_claims` (OperationalReadinessCases). Suite is NOT passing.
- Already recorded: infra home 181 pass / 0 fail / 1 skip (above) and native REST fixture 25/25 pass (above) stand. Parent all-8 repository-health checks pass.

## 7. Critical unresolved / NEEDS_REVIEW before production

Do not ship before these are genuinely fixed and verified on Linux (Mac has no Docker/Bun; local Mac tests using extracted seams/schema mimic are insufficient alone):

1. Final verifier calls F-owned manifest OPTIONAL and names `patch-manifest.json` whereas F emits `manifest.json`. Validate actual doctor/Python ALL exact post-hashes + unexpected tracked drift fail-closed; no marker-only acceptance.
2. Need actual pinned `loadConfig` and `loadTenantPackage` modules on Linux/Bun.
3. New Me tenant 5-YAML avoids fintech; bridge-only native auth `config.ts` patch prevents OAuth boot failure; same-Request header memo prevents nonce consumption on repeated guards; cross-request replay rejected. Verify with actual source modules on Linux.
4. Verify actual readiness home runtime → phone (120s singleflight / TTL / hard invalidation), model config same installer/harness env, actual CONTAINER-namespace Ollama reachability (host tags alone are not proof), per-person fleet admin probe + user 403.
5. Verify latest installer strict doctor schema matches the final contract; production requires all PASS, not fake READY.
6. Provider / license / hardware tests are genuine operator gates.
7. No live inference or real credential reads required for current source acceptance.
8. Local Mac: no Docker/Bun, disk ~1.4GB — **do not delete user OpenCode 9.5GB history/database**; Linux CI does heavy builds.

The raw historical review queue mixes fixed and unresolved items: the curated list in `agent-infrastructure-checkpoint/` labels uncertain findings `NEEDS_REVIEW` — it does not pretend all are open or all fixed.

## 8. Roadmap / resume (narrow, in order)

1. Clone pinned branch snapshots → read this doc + checkpoint contracts.
2. Inspect partial installer key-mapping/tests and patch manifest mismatch.
3. Normal OpenCode: narrowly fix genuine blockers only.
4. Run current suites + coordinated Linux acceptance (actual source config/tenant/Docker), refresh PR evidence.
5. User provides license / home Docker / K8s / provider config → actual licensed first-boot + pair 2 family + long tasks / restart / approvals / privacy-push tests.
6. Later: native artifact/push parity and full-native backup implementations, upstream update drill.
7. Do not start unrelated features.

Sources consulted (local, not copied verbatim): prior results contracts/completions/checkpoint/reviewqueue, `platform-integration-final`, `patch-auth-final`, `supervisor-production-review`, `runtime-local-default-fix` prompts, and actual repo files. Sanitized contracts preserved in `.github/docs/agent-infrastructure-checkpoint/`. Raw logs, shell env, tokens, OpenCode database/history, and private licensed config are intentionally NOT preserved.
