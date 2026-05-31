---
session_date: 2026-05-31
session_type: newp
project: QuickBasicEmulator
ecosystem: Retro_Computing
agent: Claude Code (Opus 4.7, 1M context)
resume: false
status: completed
---

# Sessie 2026-05-31 — newp QuickBasicEmulator

## Prompt-1: opdracht

> newp "QuickBasicEmulator";Web,X86, Android. Laad .bas files, compile ren run gwbasic, qbasic en quickbasic. ondersteund ook decompilen van qbasic/quickbasic .exe files.

## Fase 0 — /verifyrules

Skill `/verifyrules` aangeroepen vóór WhatIf, conform `feedback_newp_includes_verify_sanity`. Naleefmatrix: 5/12 volledig, 6/12 in afwachting van fase, 1/12 auto-gecorrigeerd (statusblok).

## Fase 1 — Begrip-terugkoppeling

Bevestigd: drie dialecten (GW-BASIC, QBasic, QuickBASIC 4.5), drie platforms (Web/X86/Android), load+compile+run + decompile QB 4.5 `.exe`. Akkoord begrip ontvangen.

## Fase 2 — Lijst beslispunten

10 beslispunten gepresenteerd: B1-ecosysteem, B2-repo-structuur, B3-visibility+licentie, B4-codenaam-thema, B5-runtime-architectuur, B6-decompiler-scope, B7-security/legal, B8-platform-prio MVP, B9-tech-stack, B10-tools-inventarisatie. Volgorde: B10→B1→B7→B3→B2→B5→B6→B8→B9→B4. Akkoord volgorde ontvangen.

## Fase 3 — Beslispunten één-voor-één (ONE_BY_ONE §6b)

### B10 — Tools-inventarisatie (WebSearch)
- QB64-PE (MIT, C++/CMake, QB 4.5 native-compat, Phoenix Edition actief 2026)
- QBJS (boxgaming/qbjs, web-IDE+runtime, vanilla JS)
- qbjc (jichu4n/qbjc, QB→JS transpiler, Node+browser)
- FreeBASIC (GPL-2.0, vermijden i.v.m. copyleft conflict met onze AGPL-publish-doelen)
- vintage-basic (Haskell, BSD, MS BASIC v2 — verkeerde dialect, niet bruikbaar)
- microsoft/GW-BASIC (MIT, 1983 8088 assembly, historical reference)
- QBDeCompiler: bestaat niet als werkende tool; `maurom/qbasic-reversing-notes` als kennisbasis
- **Beslissing:** wrap-en-uniformeer-strategie (B5-D). Akkoord drie sub-vragen ontvangen.

### B1 — Ecosysteem
**Beslissing:** Optie B — nieuw ecosysteem "Retro_Computing", sub-master `Meta_Retro_Computing`. Onderscheid t.o.v. Gaming-ecosysteem (hardware-emulatie) bewaard.

### B7 — Security/legal-policy
**Beslissing:** a-2 (MS GW-BASIC source als spec-referentie, geen verbatim port) + b-1+b-2 (eigen signature-DB + notes-fork, BYO-BRUN45) + c-1+c-3 (disclaimer + watermark in decompiler-output) + verplichte `LEGAL.md` in repo-root.

### B3 — Visibility + licentie
**Beslissing:** PUBLIC vanaf v0.0.1, AGPL-3.0, GitHub-org `cpaglebbeek/`. NOTICE.md per repo voor upstream-attributies.

### B2 — Repo-structuur
**Beslissing:** Optie B met 6 repos — Meta_QuickBasicEmulator + Core + Web + X86 + Android + Decompiler. Decompiler als eigen repo vanaf dag 1.

### B5 — Runtime-architectuur
**Beslissing:** Optie D — hybride wrap → migrate. Fase-1 wrap (QBJS-fork voor Web + QB64-PE-fork voor X86 + WebView voor Android), fase-2 migrate Core naar Rust, fase-3 unified Rust-runtime. Canonieke test-suite in `_Core/tests/`.

### B6 — Decompiler-scope MVP
**Beslissing:** Optie B — BRUN-mode eerst (v0.0.4-Whitten prototype, v0.2.0-Weiland stabiel), Stand-alone EXE-mode in v0.3.x (v0.4.0-Chien). Test-corpus discipline (`_Decompiler/tests/corpus/`) vanaf v0.1.x.

### B8 — Platform-prio MVP
**Beslissing:** Optie A — Web → X86 → Android. Live-demo-doel: icthorse.nl/quickbasic-emulator/ vanaf v0.1.0-Letwin.

### B9 — Tech-stack per platform
**Beslissing:**
- Core: TypeScript + Vitest + JSON dialect-spec
- Web: Vanilla JS fork QBJS + Vite, consumeert Core via path-dep
- X86: C++/CMake fork QB64-PE
- Android: Kotlin + Jetpack Compose + WebView
- Decompiler: Rust + serde + clap
- Core-distributie fase-1: path-dep / workspace (geen npm-publish)
- CI: GitHub Actions per repo

### B4 — Codenaam-thema
**Beslissing:** Microsoft BASIC-pioniers, v0.0.1-Gates → v1.0.0-Kemeny (laatste breekt thema voor majestueuze release). Roadmap-mapping zie ROADMAP.md.

## Fase 4 — Repo-aanmaak skeleton

7 repos aangemaakt op GitHub `cpaglebbeek/`:
- Meta_Retro_Computing (sub-master)
- Meta_QuickBasicEmulator (project-meta)
- QuickBasicEmulator_Core
- QuickBasicEmulator_Web
- QuickBasicEmulator_X86
- QuickBasicEmulator_Android
- QuickBasicEmulator_Decompiler

Lokaal aangemaakt in `/Users/christian/Documents/Gemini_Projects/`. Skeleton-content geschreven. AGPL-3.0 LICENSE in elke repo.

## Fase 5 — Meta_Master updates

PROJECTS.json, ECOSYSTEMS.md en STATUS.md bijgewerkt in Meta_Master met nieuwe ecosysteem Retro_Computing + 7 repo-entries.

## Fase 6 — Skeleton-content per repo

Universele bestanden (README, LICENSE, CLAUDE.md, .gitignore, version.json, .github/workflows/ci.yml) + repo-specifieke startbestanden.

## Fase 7 — Memory-laag

Nieuwe memories: `project_quickbasicemulator.md` en `project_meta_retro_computing.md`. MEMORY.md geüpdated. Mirror naar Meta_Master/claude_memory/.

## Fase 8 — Commit + push

Alle repos eerste commit + push naar GitHub. Meta_Master commit + push.

## Fase 9 — /sanitycheck

Deep-dive audit op zojuist aangemaakte skeleton. Bevindingen als follow-up commits.

## Fase 10 — Eindrapport

Samenvatting + links + volgende-stap-advies (v0.0.2-Allen werk starten in Core).

## Volgende stap

Bij volgende sessie: open `Meta_QuickBasicEmulator/ROADMAP.md` voor v0.0.2-Allen werk: AST-types schema + dialect-spec JSON voor alle drie dialecten + test-suite v1.
