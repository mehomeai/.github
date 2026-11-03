# Curated contracts (sanitized, 2026-11-03)

Source: prior results/*.json prompts. No secrets, tokens, logs, or machine paths.

## Pins

- OpenBot `6bfee780c8e8d05141375ea55ebde33eefb3dbc0` (CopilotKit/OpenBot)
- OpenMuse `a4944f4c471d3dc93b8bfc060b5da309b5216ab2`
- OpenDuet `f52288fff5ccdbd3de428d573d9f026fce5ff480`
- OpenDots `d4214aff10f066911f3253cb3cb870764cd51a9f`
- MIT notices / manifest tracked in-repo; exact upstream graph, no homebrew engine.

## Runtime wire (agentic-framework)

- Image `openbot-agent-langgraph-agui:6bfee780c8e8`; port 4206, POST `/` + `/health`, `x-openbot-agent-token`; provider/model/tool-callback env; readiness semantics in `runtime-wire-contract`.
- Local Ollama route (`runtime-local-model-contract-v1`): `BOT_PROVIDER=openai` (literal, `ollama` rejected), `BOT_MODEL` e.g. `llama3.1`, `OPENAI_BASE_URL=<ollama>/v1`, `OPENAI_API_KEY=ollama` documented non-secret dummy (SDK refuses keyless), `MANAGED_AGENT_TOKEN` + `AGENT_TOOL_TOKEN` required, `OPENBOT_TOOL_URL` FULL endpoint posted verbatim. Container must use host-gateway/`host.docker.internal`, never container-127 as host proof.
- Production durability delegated to licensed self-hosted Intelligence; legacy `pending.json` accessible, never silently used by full mode.

## Platform / doctor (infra-server + setup-infra)

- `platform-wire-contract rev2`: proxy bounds, capability split, push privacy, harness wiring, fleet, `BOT_*` model, tracker.
- `setup-doctor-contract (me-setup-doctor-v1 / upstream-platform-v1/full.1)`: READY only when every required evidence field present — git-anchored sourcePin (HEAD == pin, patch applied, filesOk, approved-footprint-only, manifest exact when F manifest exists), fleet per-person probes + roleGate user-denied, native model selectedServed from single `BOT_*` source, live tracker probe true, postgres live via native read, supervisor docker true, telemetry enforced, datastore perms, intelligence self-hosted + licensed + SDK `1.83.0`. Minimal forged READY rejected. States READY / NEEDS_INPUT (honest gate) / FAILED (blocks install).
- `platform-client-answers`: no WS tunnel (polling+push alternative), `configValid` vs `liveEvidence` split.
- `datastorage-contract`: Intelligence DB/Redis/events; automation unsupported explicit; `--require-backup` strict optional, operational READY excludes backup claims by decision.
- `setup-wire-contract`: exact CLI/install/doctor config schema; upstream_mode None default + sticky preserve; fresh installs default full, core = computer-plane dev/partial.
- Invocation: `node --experimental-transform-types src/cli.mjs upstream-doctor [--no-probe] [--cache <data-dir>/doctor-cache.json]`, cwd-independent (module-root lock), TTL 300s, live singleflight refresh 120s / 30s probe deadline.

## Auth patch (F-owned, infra-server vendor)

- `patch-auth-v3`: patch `vendor/openbot/patches/0001-home-identity-bridge.patch` sha256 `e6575f10…6cde87`; `config.ts` post `03fcdbbf…`, `index.ts` post `83a049d5…`, new `home-bridge.ts` post `f08df17d…`; unmodified seams `request-actor.ts` `95099b80…`, `auth/guards.ts` `20071b33…`, `auth/dev-actor.ts` `386761a8…`.
- Bridge-only boots without external OAuth; same-Request WeakMap memo (no nonce re-consume); cross-request replay refused without cookie fallback.
- Verifier must byte-verify ALL post-hashes + reject unexpected tracked drift; candidate patch-compat job must cover `config.ts` too (currently index-only). **Filename mismatch open: verifier references `patch-manifest.json`, F emits `manifest.json` — NEEDS_REVIEW.**

## Phone (phone-app)

- Native-REST-primary, verified pinned callsites; history accepts `TranscriptMessage[]` OR `{messages}`; approvals/questions real endpoints with fenced single-tap gate; idempotent outbox keyed by UUID (never text match), pending overlay until canonical echo; generic-only push privacy (no chat text to Expo/APNs); THIRD-PARTY-NOTICES with exact OpenBot MIT text, adapted-provenance ledger.
