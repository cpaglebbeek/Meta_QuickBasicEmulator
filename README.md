# Meta_QuickBasicEmulator

Project-meta voor **QuickBasicEmulator** — een multi-platform emulator/runtime + decompiler voor klassieke Microsoft BASIC-dialecten (GW-BASIC, QBasic, QuickBASIC 4.5).

> ⚠️ **v0.0.1-Gates — Skeleton.** Project net aangemaakt op 2026-05-31. Nog geen werkende runtime. Zie `ROADMAP.md` voor planning.

## Doel

| Functie | Status MVP-1 | Status MVP-2 |
|---|---|---|
| `.bas`-bestanden laden (ASCII + GW tokenized) | Web (v0.0.3) | X86 (v0.3.0) |
| GW-BASIC compileren + uitvoeren | Web (v0.0.3) | X86 (v0.3.0) |
| QBasic compileren + uitvoeren | Web (v0.0.3) | X86 (v0.3.0) |
| QuickBASIC 4.5 compileren + uitvoeren | Web (v0.0.3) | X86 (v0.3.0) |
| QB 4.5 `.exe` decompileren naar `.bas` | BRUN-mode (v0.0.4) | Stand-alone (v0.3.0) |
| Android-app | WebView (v0.6.0 Lampson) | Native (v1.0+) |

## Ecosystem

- **Ecosysteem:** Retro_Computing
- **Sub-master:** [`cpaglebbeek/Meta_Retro_Computing`](https://github.com/cpaglebbeek/Meta_Retro_Computing)
- **Master:** [`cpaglebbeek/Meta_Master`](https://github.com/cpaglebbeek/Meta_Master)

## Repos (deze project-suite)

| Repo | Tech | Doel |
|---|---|---|
| **Meta_QuickBasicEmulator** (deze) | Markdown + JSON | Project-coördinatie, architectuur, roadmap |
| [`QuickBasicEmulator_Core`](https://github.com/cpaglebbeek/QuickBasicEmulator_Core) | TypeScript + Vitest | Dialect-spec + AST-types + test-suite |
| [`QuickBasicEmulator_Web`](https://github.com/cpaglebbeek/QuickBasicEmulator_Web) | Vanilla JS + Vite (fork QBJS) | Browser-runtime |
| [`QuickBasicEmulator_X86`](https://github.com/cpaglebbeek/QuickBasicEmulator_X86) | C++/CMake (fork QB64-PE) | Native runtime Windows/Linux/macOS |
| [`QuickBasicEmulator_Android`](https://github.com/cpaglebbeek/QuickBasicEmulator_Android) | Kotlin/Compose + WebView | Android-app |
| [`QuickBasicEmulator_Decompiler`](https://github.com/cpaglebbeek/QuickBasicEmulator_Decompiler) | Rust + serde + clap | Decompiler-CLI voor QB 4.5 `.exe` |

## Architectuur in één plaatje

```
       ┌────────────────────────────┐
       │  QuickBasicEmulator_Core   │  ← TS dialect-spec + AST + test-suite
       └─────────────┬──────────────┘
                     │ (path-dep / npm-workspace)
        ┌────────────┼────────────┐
        ▼            ▼            ▼
   [_Web fork]  [_X86 fork]  [_Decompiler]
   QBJS+dialect QB64-PE+flag  Rust CLI (own AST)
        │
        ▼
   [_Android WebView wrapper rond _Web]
```

Zie `ARCHITECTURE.md` voor de fasering (fase-1 wrap → fase-2 Rust-Core migratie → fase-3 unified).

## Licentie + IP-beleid

- **Licentie:** AGPL-3.0 (zie `LICENSE` in elke repo)
- **MS-trademarks:** GW-BASIC, QBasic, QuickBASIC zijn Microsoft trademarks
- **MS source-referentie:** [`microsoft/GW-BASIC`](https://github.com/microsoft/GW-BASIC) (MIT) wordt gebruikt als **dialect-spec-referentie**, geen verbatim code-port
- **Decompiler-policy:** BYO-BRUN45 (geen MS-binaries in onze repos), reconstructed source met watermark
- Zie `LEGAL.md` voor volledige policy

## Status

| Aspect | Waarde |
|---|---|
| Huidige versie | v0.0.1-Gates |
| Aangemaakt | 2026-05-31 |
| MVP-1 deploy-doel | icthorse.nl/quickbasic-emulator/ |
| Volgende milestone | v0.0.2-Allen (Core dialect-spec + test-suite v1) |

## Documentatie

- [ARCHITECTURE.md](./ARCHITECTURE.md) — componenten + relaties cross-repo
- [ROADMAP.md](./ROADMAP.md) — codenamen v0.0.1 → v1.0.0 + milestones
- [LEGAL.md](./LEGAL.md) — IP-beleid, MS-trademarks, BYO-BRUN45-policy
- [docs/PRINCIPLES.md](./docs/PRINCIPLES.md) — Dragon1-stijl genummerd register P-QBE-01..10
- [docs/DEPENDENCIES.md](./docs/DEPENDENCIES.md) — component-deps + oorzaak-gevolg-matrix
- [BUGLIST.md](./BUGLIST.md) — bekende issues + terugkerende patronen
- [ACTIONS.md](./ACTIONS.md) — openstaande acties
- [CLAUDE.md](./CLAUDE.md) — sessie-regels
- [prompts/](./prompts/) — sessie-transcripties
