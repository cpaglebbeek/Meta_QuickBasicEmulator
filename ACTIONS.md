# QuickBasicEmulator — Openstaande Acties

Datums in DD-MM formaat. Bron tussen haakjes.

## v0.0.1 → v0.0.2 (Gates → Allen) — AFGEROND 01-06

- [x] Core: bepaal AST-types schema (sub/function/label/statement-typen) (01-06)
- [x] Core: schrijf dialect_gwbasic.json met top-50 statements (01-06, 52 statements + 33 builtins)
- [x] Core: schrijf dialect_qbasic.json met top-50 statements (01-06, 49 statements + 38 builtins)
- [x] Core: schrijf dialect_qb45.json met top-50 statements (01-06, 53 statements + 45 builtins)
- [x] Core: maak `tests/sample_*.bas` per dialect (01-06, 15 samples + 4 expected.txt)
- [x] Core: spec.test.ts met 23 consistency-checks (28/28 tests groen totaal)

## v0.0.2 → v0.0.3 (Allen → Davidoff) — AFGEROND 01-06

- [x] Web: fork QBJS in `QuickBasicEmulator_Web` (vendoring + NOTICE) (01-06, MIT licentie geverifieerd, commit e3ca41c6)
- [x] Web: voeg GW-BASIC dialect-mode toe (pre-flight: line-numbers required + verbied SUB/FUNCTION/SELECT/DO/END IF) (01-06)
- [x] Web: integreer Core test-suite via dialects.test.ts — e2e hello + loops + conditionals + procedures + arrays per dialect (01-06, 18/18 vitest groen)

## v0.0.3 → v0.0.4 (Davidoff → Whitten)

- [ ] Decompiler: Rust skeleton met clap-CLI + serde JSON-output (31-05, newp-sessie)
- [ ] Decompiler: BRUN-mode prototype op één test-EXE (31-05, newp-sessie)
- [ ] Decompiler: signature-DB-schema (JSON) (31-05, newp-sessie)
- [ ] Decompiler: BYO-BRUN45 instructie in signatures/README.md (31-05, newp-sessie)

## v0.0.4 → v0.1.0 (Whitten → Letwin) — MVP-1 LIVE

- [ ] Web: deploy build naar icthorse.nl/quickbasic-emulator/ (31-05, newp-sessie)
- [ ] Web: LiteSpeed cache purge na deploy (31-05, newp-sessie)
- [ ] Documentatie: release-notes voor v0.1.0 publieke launch (31-05, newp-sessie)
- [ ] Announcement: LinkedIn-post draft over MVP-1 (31-05, newp-sessie)

## Continue

- [ ] Test-corpus uitbreiden (Nibbles, Gorillas, Money, Donkey als ground-truth)
- [ ] BUGLIST.md bijhouden in elke repo
- [ ] CLAUDE.md per repo synced houden met cross-repo conventies

## Sanitycheck follow-ups (uit v0.0.1-Gates sessie)

- [x] P1-1 CI workflows actief in 5 platform-repos (01-06)
- [x] P2-1 docs/PRINCIPLES.md aangemaakt (01-06)
- [x] P2-2 docs/DEPENDENCIES.md aangemaakt (01-06)
- [ ] P3-1 docs/screens/ aanmaken bij v0.0.3-Davidoff (Web krijgt UI)
- [ ] P3-2 DESIGN_TOKENS.md aanmaken bij v0.0.3-Davidoff
- [ ] P3-3 CONTENT_INVENTORY.md aanmaken bij v0.0.3-Davidoff
- [ ] P3-4 CHANGELOG.md per repo aanmaken bij v0.1.0-Letwin
