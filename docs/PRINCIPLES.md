# QuickBasicEmulator — Architectuurprincipes

Dragon1-stijl genummerd register. Elk principe heeft een **stelling** + **rationale** + **toepassing**.

Codering: `P-QBE-NN`. Bij wijziging van een principe: bump versie van dit document + log in CHANGELOG.

---

## P-QBE-01 — Dialect-spec is bron van waarheid

**Stelling:** De canonieke beschrijving van GW-BASIC, QBasic en QuickBASIC 4.5 ligt in `QuickBasicEmulator_Core/src/spec/dialect_*.json`. Geen enkele runtime mag dialect-gedrag definiëren dat afwijkt van de spec.

**Rationale:** Drie platform-runtimes (Web, X86, Android) en één decompiler-output moeten consistent zijn. Eén bron voorkomt dialect-drift (zie BUGLIST.md DIALECT-DRIFT-pattern).

**Toepassing:**
- Web (QBJS-fork) en X86 (QB64-PE-fork) doorlopen beide de **canonieke test-suite** in `_Core/tests/`
- Decompiler-output gebruikt Core's AST-types
- Wijzigingen in dialect-gedrag eerst in spec, daarna in runtimes — nooit andersom

---

## P-QBE-02 — Wrap-en-uniformeer (fase 1) → Unify (fase 3)

**Stelling:** In fase 1 (v0.0.x → v0.6.x) bouwen we op bewezen open-source kernen (QBJS, QB64-PE) via fork-vendoring. In fase 2 migreren we Core naar Rust. In fase 3 (v1.0+) is Core de unified runtime en zijn de forks afgebouwd.

**Rationale:** B10-inventory toonde dat QB-compat 10+ jaar engineering vraagt. From-scratch in v0.0.x betekent jaren wachten voor werkende MVP. Wrappen levert sneller, en de test-suite + dialect-spec voorkomen dat we vast zitten in forks op de lange termijn.

**Toepassing:**
- Web: fork-import QBJS in v0.0.3-Davidoff (niet eerder)
- X86: fork-import QB64-PE in v0.3.0-Chen (niet eerder)
- Core: Rust-rewrite start v0.7.x, eindigt v1.0.0-Kemeny
- NOTICE.md per repo registreert origin + commit-hash bij fork-import

---

## P-QBE-03 — Test-suite forceert dialect-consistentie

**Stelling:** Elke platform-runtime moet de canonieke test-suite in `_Core/tests/` slagen. CI faalt bij drift.

**Rationale:** Twee onafhankelijke forks (QBJS voor Web, QB64-PE voor X86) zullen subtiele dialect-verschillen vertonen. Zonder test-discipline divergeren de runtimes. De suite is de **werkende contract-spec**.

**Toepassing:**
- `_Core/tests/samples/*.bas` per dialect + `expected/*.txt`
- Per runtime een test-driver die `.bas` → output vergelijkt met expected
- CI in elke runtime-repo refereert aan Core via path-dep
- Nieuwe spec-feature = nieuwe test-case + run in beide runtimes

---

## P-QBE-04 — Geen Microsoft-binaries in publieke repos

**Stelling:** Geen distributie van `BRUN45.EXE`, `BCOM45.LIB`, of andere Microsoft proprietary binaries in onze repos. Punt.

**Rationale:** Vermijdt elke IP-discussie. Houdt repos PUBLIC-veilig. Decompiler-signature-DB wordt **lokaal op gebruiker-machine** opgebouwd uit hun eigen QB-installatie (BYO-BRUN45).

**Toepassing:**
- `.gitignore` per repo voor bekende QB-bestandsnamen
- `QuickBasicEmulator_Decompiler/signatures/README.md` documenteert BYO-procedure
- `LEGAL.md` legt het beleid expliciet vast
- Decompiler-CLI faalt gracefully zonder signature-DB met instructies-link

---

## P-QBE-05 — Decompiler-output bevat verplicht watermark

**Stelling:** Elke door de decompiler gegenereerde `.bas` bevat een header-watermark met versie, codenaam, datum, source-filename en use-policy-waarschuwing. Watermark is **niet verwijderbaar via een vlag**.

**Rationale:** Reconstruction is approximate, niet identiek. Gebruikers moeten weten dat (a) variabelnamen/comments verloren zijn, (b) legal-verantwoordelijkheid bij hen ligt. Niet-verwijderbaar voorkomt frictieloos "doen alsof het origineel is".

**Toepassing:**
- Decompiler-template vastgelegd in `LEGAL.md`
- Code-path heeft geen `--no-watermark` of vergelijkbare bypass
- Tests verifiëren aanwezigheid watermark in output

---

## P-QBE-06 — MS GW-BASIC source = spec-referentie, geen verbatim port

**Stelling:** `microsoft/GW-BASIC` (MIT, 1983 8088 assembly) wordt gebruikt als **dialect-specificatie-referentie** voor `dialect_gwbasic.json`. Géén verbatim code-port (geen assembly→TS rewrite).

**Rationale:** MIT staat alles toe, maar verbatim port erft historische 8088-bugs en geeft "MS-look" aan onze AGPL-codebase. Spec-referentie = zelfde nut zonder bagage.

**Toepassing:**
- Attributie naar MS GW-BASIC repo in `LEGAL.md` en `_Core/src/spec/dialect_gwbasic.json` (source_reference-veld)
- AST-types en parser-impl zijn origineel werk
- Bij twijfel over statement-semantiek: MS-source raadplegen, eigen impl schrijven

---

## P-QBE-07 — Platform-prio: Web → X86 → Android

**Stelling:** Web is MVP-1 deliverable (v0.1.0-Letwin LIVE op icthorse.nl/quickbasic-emulator/). X86 is MVP-2 (v0.3.0-Chen). Android is MVP-2-tail (v0.6.0-Lampson, WebView-wrapper).

**Rationale:** Demo-deelbaarheid (URL > 50MB binary) wint adoption sneller dan native compleetheid. QBJS-fork is lichter dan QB64-PE-fork. Android krijgt Web-build "gratis" via WebView.

**Toepassing:**
- v0.0.x energie op Web + Core
- X86 skeleton blijft placeholder tot v0.3.0
- Android skeleton blijft placeholder tot v0.6.0
- Geen native Android-port vóór fase-3 (v1.0+)

---

## P-QBE-08 — Versie-bump bij elke wijziging die gepushed wordt

**Stelling:** Conform `feedback_randomringtone_versioning`: elke wijziging die naar GitHub gepushed wordt bumpt minimaal `+0.0.1` in `version.json` van de geraakte repo.

**Rationale:** Reproduceerbaarheid: een gebruiker die "v0.0.4 van Web" zegt, refereert aan een specifieke build. Geen "main" als bewegend doel.

**Toepassing:**
- Pre-push check (manueel of CI): version.json delta?
- Codenaam erft van de minor (alle v0.0.x patches blijven "Gates" tot v0.0.2-Allen)
- RELEASES.md per repo logt belangrijke releases

---

## P-QBE-09 — Deploy-discipline Web (icthorse.nl)

**Stelling:** Conform `feedback_icthorse_deploy`: Web-deploy is rsync naar Hostinger + LiteSpeed cache purge. Beide stappen verplicht, in deze volgorde.

**Rationale:** LiteSpeed cached agressief; rsync zonder purge betekent dat gebruikers oude bundle zien. Bug-pattern CACHE in BUGLIST.md.

**Toepassing:**
- Deploy-script in `_Web/scripts/deploy.sh` (vanaf v0.0.3) bevat beide stappen
- Verificatie: version-string in browser-console matcht `version.json` na deploy
- BUGLIST.md CACHE-entry tracked herhaalde incidenten

---

## P-QBE-10 — Fork-attribution in NOTICE.md

**Stelling:** Bij elke upstream-fork-import (QBJS, QB64-PE, of andere): NOTICE.md in het fork-repo registreert origin-URL, imported commit-hash, import-datum, upstream-licentie en doorgifte-licentie (AGPL-3.0).

**Rationale:** AGPL vereist source-distributie + attribution. Toekomstige refactors moeten kunnen herleiden welke commit-baseline we hebben.

**Toepassing:**
- `_Web/NOTICE.md` (vanaf v0.0.3) vult QBJS-velden
- `_X86/NOTICE.md` (vanaf v0.3.0) vult QB64-PE-velden
- `_Decompiler/NOTICE.md` verwijst naar `maurom/qbasic-reversing-notes` als kennisbasis (geen code-port)

---

## Toekomstige principes (gereserveerd)

- P-QBE-11 — Signature-DB-schema-stabiliteit (v0.4.0-Chien moment)
- P-QBE-12 — Native Android-port-criteria (fase-3 moment)
- P-QBE-13 — Core Rust-migratie-volgorde (fase-2 moment)

## Relaties met Meta_Master-principes

| Meta_Master principe | Verhouding tot QBE |
|---|---|
| P-AGT-04 (Multi-Session Safety) | QBE volgt: pull voor wijziging, status check |
| P-TECH-04 (Shared Infrastructure) | QBE deploy raakt icthorse.nl-shared-resources |
| P-DAT-XX (Expliciete vastlegging) | QBE volgt: dit document + DEPENDENCIES.md + ARCHITECTURE.md |
