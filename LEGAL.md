# QuickBasicEmulator — LEGAL.md

## Trademarks

GW-BASIC™, QBasic™ en QuickBASIC™ zijn handelsmerken van Microsoft Corporation. Dit project gebruikt deze namen uitsluitend ter aanduiding van de **dialecten** die het ondersteunt, conform fair use. Dit project is **niet gelieerd aan Microsoft** en wordt niet door Microsoft ondersteund of goedgekeurd.

## Bron-referenties

| Bron | Licentie | Hoe gebruikt | Distributie in onze repos |
|---|---|---|---|
| [`microsoft/GW-BASIC`](https://github.com/microsoft/GW-BASIC) | MIT | **Spec-referentie** voor GW-BASIC dialect-gedrag (tokens, statement-semantiek). **Geen verbatim code-port.** | Geen MS-code in onze repos |
| [`boxgaming/qbjs`](https://github.com/boxgaming/qbjs) | TBD bij fork (verwacht: open) | Geforkt in `QuickBasicEmulator_Web` (v0.0.3+) | Volledig vendored conform fork-licentie + AGPL-3.0 doorgifte |
| [`QB64-Phoenix-Edition/QB64pe`](https://github.com/QB64-Phoenix-Edition/QB64pe) | MIT (Phoenix repos) | Geforkt in `QuickBasicEmulator_X86` (v0.3.0+) | Volledig vendored conform fork-licentie + AGPL-3.0 doorgifte |
| [`maurom/qbasic-reversing-notes`](https://github.com/maurom/qbasic-reversing-notes) | TBD bij gebruik | Kennisbasis voor decompiler signature-DB | Geen verbatim hergebruik, alleen techniek-leerstof |

Verbatim attribution + NOTICE.md in de respectievelijke repos (`_Web`, `_X86`, `_Decompiler`).

## Decompiler-policy

### BYO-BRUN45

De decompiler benodigt voor full Stand-alone EXE-mode (v0.4.0+) een **signature-DB** van bekende QuickBASIC 4.5 runtime-stubs. Wij distribueren **geen Microsoft-binaries** in onze repos.

Gebruikers worden gevraagd zelf hun legitieme kopie van `BRUN45.EXE` / `BCOM45.LIB` (uit hun eigen QuickBASIC 4.5 installatie) lokaal aan te leveren. De decompiler bouwt dan de signature-DB **lokaal op de machine van de gebruiker** op, eenmalig. De DB is niet ge-commit naar onze repo.

Zie `QuickBasicEmulator_Decompiler/signatures/README.md` voor details.

### Use-policy + disclaimer

Decompilatie van software valt onder auteursrecht-uitzonderingen die per jurisdictie verschillen. Dit project biedt een **technische capability** voor:

- ✅ **Eigen verloren source-code** herstellen (legitiem in vrijwel alle jurisdicties)
- ✅ **Interoperabiliteit-analyse** (in EU toegestaan onder Software Directive Art. 6)
- ✅ **Educatief begrip** van QB-compiler-uitvoer
- ⚠️ **Reverse-engineering van andermans propriëtaire software** is je **eigen juridische verantwoordelijkheid** om te checken in jouw jurisdictie

### Watermark in output

Decompiler-output (gereconstrueerde `.bas`-bestanden) bevat altijd een **header-watermark**:

```
' Reconstructed by QuickBasicEmulator vX.Y.Z (Codename)
' Source: <input-filename>
' Date: <YYYY-MM-DD>
' WARNING: Reconstructed source is approximate. Original variable names,
'          comments and indentation are lost. Verify your legal right to
'          decompile the input executable. See LEGAL.md.
```

Dit watermark is **niet verwijderbaar via een vlag** (zou ondermijnend zijn voor het kader).

### Disclaimer

DIT PROJECT WORDT GELEVERD "AS IS", ZONDER WARRANTY, expliciet of impliciet. De auteur(s) aanvaarden geen aansprakelijkheid voor enig gebruik van dit project of de output ervan, inclusief gevolgen van decompilatie-acties. Zie ook AGPL-3.0 LICENSE.

## Reverse-engineering-ethiek

Onze decompiler herkent **functionele fingerprints** (byte-patterns) van bekende QB 4.5 runtime-functies. Deze fingerprints zijn:

- Vergelijkbaar met antivirus-signatures of Ghidra's FLIRT-bibliotheek
- Niet auteursrechtelijk beschermd als feitelijke statistische uitkomsten van publieke compiler-output
- **Niet gegenereerd door MS-binaries in onze repo te plaatsen** — gebruikers bouwen de DB lokaal op uit hun eigen installaties

## Vragen

Bij twijfel over rechtenkwesties: contact via GitHub Issues op `cpaglebbeek/Meta_QuickBasicEmulator`.
