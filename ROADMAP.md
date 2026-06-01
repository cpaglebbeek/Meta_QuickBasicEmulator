# QuickBasicEmulator Roadmap

Codenaam-thema: **Microsoft BASIC-pioniers** (1975-1991), behalve v1.0.0 (Kemeny, BASIC's vader, breekt thema voor majestueuze release).

## Versies + codenamen

| Versie | Codenaam | Wie | Milestone |
|---|---|---|---|
| **v0.0.1** | **Gates** | Bill Gates — co-author Altair BASIC (1975) | ✅ Skeleton: 6 repos + CI |
| **v0.0.2** | **Allen** | Paul Allen — co-author Altair BASIC | ✅ Core: 35 AST-Statement-types + 3 dialect-specs (52+49+53 statements + 33+38+45 builtins) + 28/28 tests groen + 15 sample-programs |
| **v0.0.3** | **Davidoff** | Monte Davidoff — math-package Altair BASIC | ✅ Web: QBJS vendored (commit e3ca41c6, MIT) + dialect-adapter + GW/QB/QB45 pre-flight modes + 18/18 tests groen + 6.4MB build |
| **v0.0.4** | **Whitten** | Greg Whitten — chief architect MS BASIC '80s, QuickBASIC design | ✅ Decompiler: PE/MZ parser + BRUN-detector (heuristic) + signature-DB schema v1.0 + Rust AST + watermark (P-QBE-05) + 16/16 tests groen, synthetic fixtures (geen MS-binaries) |
| **v0.1.0** | **Letwin** | Gordon Letwin — co-designer GW-BASIC, OS/2 architect | ✅ **MVP-1 LIVE op icthorse.nl/quickbasic-emulator/** — Web build 6.4MB gedeployed, dialect-switcher + QBJS IDE bereikbaar (+ 6 patches v0.1.1-v0.1.7: file-loader, runtime-warnings, share-URL, dual Run, limit-notice, Core matrix) |
| **v0.2.0** | **Weiland** | Ric Weiland — porteerde BASIC naar 8080/6502 | ✅ **Classic-to-structured transformer** in Web — herschrijft veilige GOSUB-RETURN blocks naar SUB procedures (K2026C-style draaibaar). Core v0.0.3 runtime_capability_qbjs.json matrix (38f+7p+11n). 43+52 tests groen. |
| **v0.3.0** | **Chen** | David Chen — QuickBASIC compiler-team | ✅ **X86 native runtime LIVE** — QB64-PE als git submodule (commit 722b7d99 MIT), wrapper CLI compileert, **K2026C draait native** (1.4MB binary, alle GOTO/GOSUB intact, geen runtime-errors zoals in QBJS) |
| v0.4.0 | Chien | (alternatief Chen-variant — TBD bij release) | Decompiler: Stand-alone EXE mode |
| **v0.5.0** | **Hopper** | Grace Hopper — compiler-pionier (eerbetoon ondanks niet MS-direct) | ✅ **F1+F2 (versmolten in v1.0.0)** — qbe-runner backend + Docker sandbox compile + Xvfb/x11vnc/noVNC stream + nginx ws-proxy + mouse/keyboard round-trip. Commits `f0afe61` → `08ec8b7`. Functioneel doorgerold naar v1.0.0-Kemeny. |
| v0.6.0 | Lampson | Butler Lampson — ACM Turing 1992 | Android WebView APK |
| ... | ... | ... | tussenversies naar behoefte |
| **v1.0.0** | **Kemeny** | John Kemeny — co-author BASIC (Dartmouth 1964) | ✅ **LIVE 01-06** op `horsecloud55.ddns.net/basic/native/` — native compiler-as-a-service (QB64-PE op HC55, Docker sandbox + Xvfb + noVNC stream + WSS keyboard/mouse). Legacy DOS-BASIC werkt 100% (K2026.BAS compileert + draait native in browser). **F1+F2** versmolten met v0.5.0-Hopper commits (`f0afe61`/`08ec8b7`). **F3** = frontend-form `/basic/native/`. **F4 production** (`6d5f0ed`) = UX error-handling + 429/400/413 + rate-limit 20/h per IP + dangerous-keyword filter + sample-dropdown + auto-stop beforeunload. **F5** sound = post-MVP. **F6 security** (`fff4460`) = gVisor runsc + cap-drop=ALL + no-new-privileges + audit-log met source-sha256. Aanleiding REDESIGN: QBJS-token-patch-loop bewees fundamentele incompat met DOS-BASIC compound-syntax. |

## Codenaam-toekenning-regel

Eén codenaam per **minor/major release** (v0.X.Y waarbij X bump). Patches (v0.X.Y bump) erven de codenaam van hun X-versie.

Bij elke release: bump `version.json` in alle 6 repos én voeg een korte release-note toe (RELEASES.md per repo).

## Geplande fases (zie ARCHITECTURE.md voor architectuur-detail)

| Fase | Versies | Aandacht |
|---|---|---|
| **MVP-1** | v0.0.1 → v0.1.0 | Skeleton + Web-runtime live |
| **MVP-2** | v0.2.0 → v0.6.0 | Decompiler stabiel + X86 + Android WebView |
| **Release** | v0.7.0 → v1.0.0 | Polish + Core Rust-rewrite + native Android optioneel |
