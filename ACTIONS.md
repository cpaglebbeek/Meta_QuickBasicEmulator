# QuickBasicEmulator — Openstaande Acties

Datums in DD-MM formaat. Bron tussen haakjes.

## v0.0.1 → v0.0.2 (Gates → Allen)

- [ ] Core: bepaal AST-types schema (sub/function/label/statement-typen) (31-05, newp-sessie)
- [ ] Core: schrijf dialect_gwbasic.json met top-50 statements (31-05, newp-sessie)
- [ ] Core: schrijf dialect_qbasic.json met top-50 statements (31-05, newp-sessie)
- [ ] Core: schrijf dialect_qb45.json met top-50 statements (31-05, newp-sessie)
- [ ] Core: maak `tests/sample_*.bas` per dialect (31-05, newp-sessie)

## v0.0.2 → v0.0.3 (Allen → Davidoff)

- [ ] Web: fork QBJS in `QuickBasicEmulator_Web` (vendoring + NOTICE) (31-05, newp-sessie)
- [ ] Web: voeg GW-BASIC dialect-mode toe (line-numbers + tokenized parser) (31-05, newp-sessie)
- [ ] Web: integreer Core test-suite (Vitest runt sample.bas-bestanden door Web-runtime) (31-05, newp-sessie)

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
