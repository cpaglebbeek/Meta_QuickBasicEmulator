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

## v0.0.3 → v0.0.4 (Davidoff → Whitten) — AFGEROND 01-06

- [x] Decompiler: Rust skeleton met clap-CLI + serde JSON-output (01-06, library + binary target)
- [x] Decompiler: BRUN-mode prototype (heuristic: BRUN45-marker + size-range, 4 confidence-levels) (01-06, synthetic fixtures i.p.v. echte QB45.exe per P-QBE-04)
- [x] Decompiler: signature-DB-schema (JSON v1.0) (01-06, schema.json + Rust SignatureDb + roundtrip-test)
- [x] Decompiler: BYO-BRUN45 instructie in signatures/README.md (01-06, CLI + Rust-API voorbeelden toegevoegd)
- [x] Decompiler: PE/MZ header parser (01-06, 28-byte minimum + e_lfanew/PE-magic detector)
- [x] Decompiler: watermark per P-QBE-05 (01-06, niet-uitschakelbaar, 3 tests)
- [x] Decompiler: Rust AST mirror van Core types (01-06, src/ast.rs + to_bas() renderer)

## v0.0.4 → v0.1.0 (Whitten → Letwin) — MVP-1 LIVE — AFGEROND 01-06

- [x] Web: deploy build naar icthorse.nl/quickbasic-emulator/ (01-06, rsync via Hostinger SSH alias)
- [x] Web: LiteSpeed cache purge na deploy (01-06)
- [x] Documentatie: release-notes voor v0.1.0 publieke launch (01-06, in STATUS.md + ROADMAP)
- [ ] Announcement: LinkedIn-post draft over MVP-1 (01-06, naar v0.2.0-Weiland of aparte sessie)

## Continue

- [ ] Test-corpus uitbreiden (Nibbles, Gorillas, Money, Donkey als ground-truth)
- [ ] BUGLIST.md bijhouden in elke repo
- [ ] CLAUDE.md per repo synced houden met cross-repo conventies

## v0.2.0-Weiland (geplande backlog uit K2026C-test 01-06)

- [ ] Classic-to-structured transformer (`_Web/src/transform/classic-to-structured.ts`): herken GOSUB-label blocks → genereer SUB-equivalent, GOTO eindewhile → restructure naar EXIT-WHILE. Maakt K2026C-stijl programs draaibaar in QBJS zonder X86-runtime. Effort: 4-8u.
- [ ] runtime_capability_qbjs.json matrix in `_Core/spec/`: machine-readable feature-coverage per runtime. Pre-flight pakt dit op.
- [ ] Decompiler v0.2.0-Weiland: BRUN-mode stabiel — echte p-code parser i.p.v. heuristic.

## Sanitycheck follow-ups (uit v0.0.1-Gates sessie)

- [x] P1-1 CI workflows actief in 5 platform-repos (01-06)
- [x] P2-1 docs/PRINCIPLES.md aangemaakt (01-06)
- [x] P2-2 docs/DEPENDENCIES.md aangemaakt (01-06)
- [ ] P3-1 docs/screens/ aanmaken bij v0.0.3-Davidoff (Web krijgt UI)
- [ ] P3-2 DESIGN_TOKENS.md aanmaken bij v0.0.3-Davidoff
- [ ] P3-3 CONTENT_INVENTORY.md aanmaken bij v0.0.3-Davidoff
- [ ] P3-4 CHANGELOG.md per repo aanmaken bij v0.1.0-Letwin
