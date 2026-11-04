# Agent Infrastructure — Resolution Addendum (2026-11-03)

Companion to `agent-infrastructure-handoff-2026-11-03.md`. The handoff and its
checkpoint remain the historical record of the paused state; this addendum
records how each open item was resolved when work resumed in the GitHub lane
(parent agent + subagents, Codex GPT-6.1 as final decision-maker, decisions
D1–D6 ratified 2026-11-03).

## Merged to main (2026-11-03, squash, dependency order)

- infra-server PR #3, setup-infra PR #2, agentic-framework PR #2,
  backend-server PR #2, phone-app PR #7 — the full
  `feat/upstream-agent-infrastructure` wave.
- web-app PR #2 — its `apps/web` + `apps/demo` + `packages/*` layout is the
  canonical frontend structure (D2).

## Open review items — resolved

1. **Manifest filename / post-hash verification** — Both verifiers
   (infra-server `src/upstream-doctor.mjs`, `scripts/upstream-openbot.py`) now
   load the authoritative `vendor/openbot/patches/manifest.json`, fail closed
   on a missing/invalid manifest, verify `postSha256` + `unmodified` digests,
   and check tracked drift. The emitter was not renamed. Checkpoint contracts
   corrected (`.github` f7a99db). Doctor tests 27/27, Python verifier 17/17.
2. **Pinned `loadConfig` / `loadTenantPackage` on Linux** — Proven under
   **Bun** (1.4.4 here; upstream pins 1.4.3) against the real patched checkout
   (pin 6bfee78) and the real tenant package (`me-home`, checksum d45090c…).
   They cannot run under Node (upstream uses extensionless TS imports;
   Bun-only by construction). A permanent proof ships as
   `scripts/openbot-bun-proof.ts` (`npm run test:upstream-bun`) with CI steps;
   the old seam test that silently swallowed the import failure now fails
   loudly. Also fixed: `apply.sh` relative-path invocation bug.
3. **Installer key-mapping / failing suites** — `model_profile` production
   bug (service.env trailing comment misparsed) fixed in setup-infra f30b11b
   with a shell-style value parser; NativeKey fixture finished via the
   production renderer. DoctorIntegrity's Mac failure does not reproduce on
   Linux (environment difference).
4. **Readiness handshake** — Implemented per
   `infra-server/docs/readiness-handshake-design.md`: two-tier live tracker
   (light 120 s; full 600 s, TTL 900 s), `live` only when both tiers pass,
   invalidation bus (generation counter; TTL/failure/cache/config-fingerprint
   triggers), `GET /readyz/full`, container-namespace model proof that is
   fail-closed (no READY until a container-namespace surface is wired).
5. **Strict doctor schema** — Supervisor validates the full nested contract
   (forged READY fails; 16 negative tests); the JS doctor stamps
   `doctor_contract: upstream-platform-v1/full.1` and shares probes via
   `src/upstream-probes.mjs`. Accepted choices (D1): `telemetry.enforced`
   stays a descriptive string; datastore permission entries only for existing
   dirs; readiness intelligence stays doctor-authoritative; the doctor's local
   Ollama block stays separate from the tracker probe (namespace semantics
   conflict, pinned by tests on both sides).
6. **Provider / license / hardware gates** — Unchanged by design; see below.

## Suite board at merge (Linux, this workspace)

infra-server: test:home 200 tests / 199 pass / 1 pre-existing skip,
test:upstream 96/96 · setup-infra: supervisor 72, setup 26, lifecycle 30
(1 skip) · backend-server 39/39 · phone-app 147/147 · agentic-framework 143
passed / 1 skipped (clean proxy env; this VM's `NO_PROXY` bracketed IPv6
entries break httpx URL parsing in the runtime client — environment artifact,
documented in the workspace notes). GitHub CI green on all merged PRs,
including infra-server Repository health after scoping its whitespace gate to
non-vendored files (vendored patch bytes are hash-pinned and must not be
reformatted).

## Still gated on the owner (D5: done-with-gate)

- **Licensed CopilotKit Intelligence E2E** — needs the license token, provider
  keys, and a Kubernetes/Docker host. The gate is enforced in code; fixtures
  and the REST-fixture acceptance cover the unlicensed scope.
- **Live container verification** — readiness namespace proof and the compose
  stack need a Docker host (none in the build VM); Ollama-dependent tests
  likewise.
- **Real hardware / devices** — phone pairing and Me Home hardware checks.

## Frontend convergence (D6)

Replit remains the frontend workshop; GitHub is the source of truth. The
first Replit → GitHub sync pilot starts once the current Replit round's
end report (change inventory + test counts) lands, importing into the
canonical `apps/*` layout via a sync branch + PR.
