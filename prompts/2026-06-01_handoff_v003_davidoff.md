---
date: 2026-06-01
repo: Meta_QuickBasicEmulator
status: open
resume: "v0.0.3-Davidoff: QBJS-fork in _Web + GW-BASIC dialect-mode + e2e hello-world"
---

# Hand-off — v0.0.3-Davidoff start in verse sessie

## Status bij hand-off (01-06)

Vorige sessie (31-05 → 01-06) leverde v0.0.1-Gates en v0.0.2-Allen op. Alles gepushed, 28/28 tests groen, sanitycheck-score ~95%. Hand-off naar verse sessie omdat fork-vendoring een eigen context-frame verdient.

## v0.0.3-Davidoff scope (akkoord 01-06)

Negen stappen:

1. **Licentie verifiëren** — clone `boxgaming/qbjs` en lees LICENSE
   ```bash
   git clone https://github.com/boxgaming/qbjs.git /tmp/qbjs-source
   cat /tmp/qbjs-source/LICENSE
   ```
   Compat-check tegen AGPL-3.0-doorgifte. **Bij conflict: STOP en terugkoppel** (akkoord 01-06 gebruiker).

2. **Fork-import vendored** — clone main in `_Web/src/qbjs-fork/`, behoud commit-hash voor NOTICE

3. **NOTICE.md vullen** in `QuickBasicEmulator_Web/NOTICE.md`:
   - Origin URL
   - Imported commit-hash (uit stap 2)
   - Import datum: YYYY-MM-DD
   - Upstream licentie
   - Doorgifte: AGPL-3.0-or-later

4. **Dialect-adapter bridge** — `_Web/src/dialect-adapter.ts` die Core's `dialect_*.json` leest en QBJS-interne dialect-API instelt

5. **GW-BASIC dialect-mode patches** in `_Web/patches/gwbasic/`:
   - Line-number-enforcement
   - SUB/FUNCTION fail-met-foutmelding (niet ondersteund in GW)
   - SELECT CASE / DO LOOP fail-met-foutmelding
   - Documenteer per patch de intent

6. **Build + dev-server werkend**: `npm install && npm run dev` toont QBJS-IDE met dialect-switcher (3 opties)

7. **Eerste e2e test** — voer `_Core/tests/samples/hello_*.bas` door elk in QBJS, vergelijk met `_Core/tests/samples/expected/hello.txt`

8. **Versie-bump**: `_Web/version.json` + `_Web/package.json` → `0.0.3-Davidoff`

9. **Meta + memory sync**:
   - `Meta_QuickBasicEmulator/version.json` → 0.0.3
   - `ROADMAP.md` v0.0.3 ✅ vinkje
   - `ACTIONS.md` v0.0.3 items afvinken
   - `~/.claude/projects/-Users-christian/memory/project_quickbasicemulator.md` status-update
   - `Meta_Master/claude_memory/` mirror update
   - Commit + push 3 repos (Web + Meta_QBE + Meta_Master)
   - Markeer deze sessie-MD `status: done` en `resume: ""` → run `update_resume.py`

## Risico-mitigatie

- **QBJS licentie incompatible** → STOP, terugkoppel naar gebruiker met opties (qbjc, from-scratch TS)
- **Upstream patches blokken upstream-updates** → documenteer in `_Web/patches/README.md` met patch-intent
- **Test-integratie te complex voor v0.0.3** → beperk tot hello-world e2e, loops/conditionals/procedures uitstellen naar v0.0.4-Whitten/v0.1.0-Letwin

## Context bij start verse sessie

- Werkdir: `/Users/christian/Documents/Gemini_Projects/QuickBasicEmulator_Web/`
- Core consumeerbaar via path-dep: `file:../QuickBasicEmulator_Core` (al in package.json)
- AST-types reeds in `_Core/src/ast/types.ts` (v0.0.2-Allen)
- Dialect-specs reeds in `_Core/src/spec/dialect_*.json`
- Test-samples reeds in `_Core/tests/samples/`
- Architectuur: B5-D hybride, P-QBE-02 (wrap-en-uniformeer) leidend
- Legal: zie `Meta_QuickBasicEmulator/LEGAL.md` voor NOTICE-template
- Principes: zie `Meta_QuickBasicEmulator/docs/PRINCIPLES.md` (P-QBE-01..10)
- Dependencies: zie `Meta_QuickBasicEmulator/docs/DEPENDENCIES.md`

## Acties voor verse sessie

1. `/checkresume` — deze sessie-MD verschijnt in RESUME.md top-10 als open
2. Kies dit nummer
3. WhatIf-terugkoppeling op deze hand-off geven
4. Vraag akkoord om stap 1 (licentie-check) te starten
5. Doorlopen volgens 9-stappen-plan

## Volgende milestone na v0.0.3-Davidoff

v0.0.4-Whitten: decompiler-BRUN-mode prototype in `_Decompiler`. Onafhankelijk van Web — kan parallel.

v0.1.0-Letwin: **MVP-1 LIVE** op icthorse.nl/quickbasic-emulator/. Vereist v0.0.3 + minimaal werkende QB-runtime.
