# QuickBasicEmulator Roadmap

Codenaam-thema: **Microsoft BASIC-pioniers** (1975-1991), behalve v1.0.0 (Kemeny, BASIC's vader, breekt thema voor majestueuze release).

## Versies + codenamen

| Versie | Codenaam | Wie | Milestone |
|---|---|---|---|
| **v0.0.1** | **Gates** | Bill Gates — co-author Altair BASIC (1975) | ✅ Skeleton: 6 repos + CI |
| **v0.0.2** | **Allen** | Paul Allen — co-author Altair BASIC | ✅ Core: 35 AST-Statement-types + 3 dialect-specs (52+49+53 statements + 33+38+45 builtins) + 28/28 tests groen + 15 sample-programs |
| **v0.0.3** | **Davidoff** | Monte Davidoff — math-package Altair BASIC | ✅ Web: QBJS vendored (commit e3ca41c6, MIT) + dialect-adapter + GW/QB/QB45 pre-flight modes + 18/18 tests groen + 6.4MB build |
| v0.0.4 | Whitten | Greg Whitten — chief architect MS BASIC '80s, QuickBASIC design | Decompiler: BRUN-mode prototype |
| **v0.1.0** | **Letwin** | Gordon Letwin — co-designer GW-BASIC, OS/2 architect | **Web MVP-1 LIVE op icthorse.nl/quickbasic-emulator/** |
| v0.2.0 | Weiland | Ric Weiland — porteerde BASIC naar 8080/6502 | Decompiler: BRUN-mode stabiel |
| v0.3.0 | Chen | David Chen — QuickBASIC compiler-team | X86: fork QB64-PE + dialect-flag |
| v0.4.0 | Chien | (alternatief Chen-variant — TBD bij release) | Decompiler: Stand-alone EXE mode |
| v0.5.0 | Hopper | Grace Hopper — compiler-pionier (eerbetoon ondanks niet MS-direct) | X86 stabiel |
| v0.6.0 | Lampson | Butler Lampson — ACM Turing 1992 | Android WebView APK |
| ... | ... | ... | tussenversies naar behoefte |
| **v1.0.0** | **Kemeny** | John Kemeny — co-author BASIC (Dartmouth 1964) | **Release: alle 3 dialecten × 3 platforms + decompiler beide modes** |

## Codenaam-toekenning-regel

Eén codenaam per **minor/major release** (v0.X.Y waarbij X bump). Patches (v0.X.Y bump) erven de codenaam van hun X-versie.

Bij elke release: bump `version.json` in alle 6 repos én voeg een korte release-note toe (RELEASES.md per repo).

## Geplande fases (zie ARCHITECTURE.md voor architectuur-detail)

| Fase | Versies | Aandacht |
|---|---|---|
| **MVP-1** | v0.0.1 → v0.1.0 | Skeleton + Web-runtime live |
| **MVP-2** | v0.2.0 → v0.6.0 | Decompiler stabiel + X86 + Android WebView |
| **Release** | v0.7.0 → v1.0.0 | Polish + Core Rust-rewrite + native Android optioneel |
