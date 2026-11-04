# Known-open review list (curated, 2026-11-03)

Labels: OPEN = must fix/verify before production. NEEDS_REVIEW = uncertain from raw history (mixes fixed and unresolved) — re-verify on Linux, do not assume open or fixed.

## OPEN

1. Manifest filename/hashes: verifier OPTIONAL + wrong name (`patch-manifest.json` vs F `manifest.json`); require ALL exact post-hashes + unexpected tracked drift fail-closed. (OPEN)
2. Pinned `loadConfig` / `loadTenantPackage` must run on Linux/Bun; Mac seam tests alone insufficient. (OPEN)
3. Installer partial key-mapping/effective-key tests interrupted by freeze; checkpointed, not finished. (OPEN)
4. Readiness handshake: live singleflight/TTL/hard invalidation home→phone; same installer/harness model env; CONTAINER-namespace Ollama proof; fleet admin probe + user 403. (OPEN)
5. Strict doctor schema vs final contract match; production all-PASS not fake READY. (OPEN)
6. Provider/license/hardware remain genuine operator gates; no live inference needed for source acceptance. (OPEN)

## NEEDS_REVIEW (re-verify; raw queue mixed fixed/unresolved)

- A: sparse checkout must verify remote-downloaded files; updater needs candidate-source-test isolation (report-only is not candidate-tested); proxy bounds (timeout/abort/size, hop-by-hop filter, no forged X-Me-*, signing secret, segment allowlist, no private callback exposure); push stays generic-only, no duplicate legacy+native; FULL mode must not invoke legacy broker for new work; doctor module-root resolution; Anthropic read-only `/v1/models` check; compose worker path + NODE_ENV + harness port 4206 + canonical image; env_file configurability/0600; Helm explicit kube-context + chart schema render + single-node profile; model single-source BOT_*; doc `PICKED_HARNESS_*` names vs consumed env + `resolveAgentTarget` 4201/ag-ui fallback. (NEEDS_REVIEW)
- B: vendor provenance vs self-hash; wrong-token (401/403) vs wrong-path test; env isolation; VENDOR.md/MANIFEST accuracy + tracked MIT; constraints actually consumed in image; tool-callback URL single-attachment; CMD strings; health semantic assert; sqlite dev-only vs image-native; Python 3.12 pin. (NEEDS_REVIEW)
- C: bounded runner/deadlines/signals/residual-group reap; session-ID streaming capture; doctor closure anchoring (entrypoint hash alone gameable) + post-run re-verify; cwd contract; strict READY schema (minimal forged READY rejected); cache handshake with A (no missing-cache fullConfigured=false forever, TTL refresh). (NEEDS_REVIEW)
- D: SDK 1.70.1 vs runtime 1.83 compat via actual types; mounted provider/screens required; notification scope + no duplicate legacy path + tap validation; two-shape history; async scope fencing (channel stamp, ABA gate release, getClients gate leak); default-full selection; bounded foreground polling/reconnect; no definite no-execution claim on timeout; tool payloads not rendered as answers; MIT notice tracked; stable id mapping (externalId != message id; inspect dispatcher/receipt). (NEEDS_REVIEW)
