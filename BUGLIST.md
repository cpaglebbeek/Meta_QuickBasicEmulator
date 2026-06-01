# QuickBasicEmulator — BUGLIST.md

## Status

Project is op v0.0.1-Gates (skeleton). Nog geen bekende bugs in code — er is nog geen code.

## Terugkerende patronen (om te ontwijken vooraf)

### CACHE (cross-repo patroon)

- **Pattern**: na Web-deploy naar icthorse.nl moet **altijd** LiteSpeed cache purge. Anders ziet gebruiker oude versie.
- **Preventieregel**: deploy-script bevat verplicht cache-purge-stap.
- **Bron**: `Meta_Master/BUGS_GLOBAL.md`, sectie CACHE.

### DEPLOY (cross-repo patroon)

- **Pattern**: rsync naar Hostinger faalt soms bij Unicode-bestandsnamen.
- **Preventieregel**: vóór deploy `find dist/ -name '*[^[:ascii:]]*'` scan.

### DIALECT-DRIFT (project-specifiek vooraf gesignaleerd)

- **Pattern**: dezelfde `.bas` levert in Web (QBJS-fork) andere output dan in X86 (QB64-PE-fork) door dialect-interpretatie-verschillen.
- **Preventieregel**: canonieke test-suite in `_Core/tests/` moet door **beide** runtimes geslagen worden vóór release. CI faalt bij drift.
- **Aanleiding**: B5 hybride-architectuur-keuze; geanticipeerd risico.

### BYO-BRUN45 (decompiler-specifiek vooraf gesignaleerd)

- **Pattern**: gebruiker installeert decompiler maar heeft geen QB 4.5 installatie → signature-DB kan niet gebouwd → Stand-alone-mode werkt niet → frustratie.
- **Preventieregel**: decompiler-CLI controleert bij start of signature-DB bestaat; geeft duidelijke foutmelding met instructies naar `signatures/README.md`.

## Bekende bugs

Geen.

## Closed bugs

Geen.

## Template per bug

Zie `Meta_Master/templates/BUGLIST_TEMPLATE.md`.

## QBJS-RUNTIME-GAP (gevonden 01-06 bij K2026C test)

- **Pattern**: QBJS-runtime ondersteunt geen classic GOTO/GOSUB/RETURN. Onze `dialect_qbasic.json` markeert ze als supported (klopt voor echte QBasic), maar QBJS-runtime negeert ze met `WARN ignoring line` → cascade `WEND without WHILE` / `IF without END IF` errors.
- **Voorbeeld**: K2026C.BAS (Theo's kalender) gebruikt GOTO eindewhile + GOSUB menu-dispatcher. QBJS kan dit niet draaien.
- **Pre-flight-implicatie**: v0.0.3-Davidoff dialect-adapter detecteert dit NIET want het is een runtime-cap, niet dialect-cap.
- **Voorgestelde fix v0.0.5+**: nieuwe `_Core/spec/runtime_capability_qbjs.json` met QBJS-feature-coverage-matrix. Pre-flight breidt uit met runtime-aware warnings.
- **Workaround**: gebruik `QuickBasicEmulator_X86` (v0.3.0-Chen, fork QB64-PE) voor classic-BASIC programs met GOTO/GOSUB.
