# QuickBasicEmulator — Architectuur

## Niveau 1: Conceptueel

```
USER
 ├── laadt .bas (GW-BASIC, QBasic, of QuickBASIC 4.5 syntax)
 │     → Web of X86 runtime
 │         → compileert + voert uit
 │             → scherm-output, sound, input-events
 │
 └── laadt .exe (QuickBASIC 4.5 compiled)
       → Decompiler-CLI
           → produceert reconstructed .bas (met watermark)
```

## Niveau 2: Logisch (component-relaties)

```
┌──────────────────────────────────────────────────────────┐
│                  Core (TS-package)                       │
│  ├── spec/dialect_gwbasic.json                           │
│  ├── spec/dialect_qbasic.json                            │
│  ├── spec/dialect_qb45.json                              │
│  ├── ast/types.ts                                        │
│  └── tests/  (canonieke test-suite, gedeeld)             │
└──────┬────────────────────┬──────────────────────────────┘
       │ npm path-dep        │ JSON-spec consumeren
       │                     │
       ▼                     ▼
┌─────────────────┐  ┌─────────────────┐  ┌────────────────┐
│ _Web            │  │ _X86            │  │ _Decompiler    │
│ fork QBJS       │  │ fork QB64-PE    │  │ Rust CLI       │
│ + dialect-mode  │  │ + dialect-flag  │  │ → output:      │
│                 │  │                 │  │   .bas (+wmrk) │
│ deployt naar    │  │ native binaries │  │   .json (AST)  │
│ icthorse.nl     │  │ Win/Linux/Mac   │  │                │
└────┬────────────┘  └─────────────────┘  └────────────────┘
     │ build-output (HTML+JS+CSS)
     ▼
┌─────────────────┐
│ _Android        │
│ Kotlin/Compose  │
│ + WebView       │
│ (assets/web/)   │
└─────────────────┘
```

## Niveau 3: Fysiek (deployment + runtime)

### Web
- Bron: `_Web/src/`
- Build: `vite build` → `_Web/dist/`
- Deploy: rsync naar Hostinger `~/domains/icthorse.nl/public_html/quickbasic-emulator/` + LiteSpeed cache purge
- Conform `feedback_icthorse_deploy`

### X86
- Bron: `_X86/src/` (C++)
- Build: `cmake --build` → native binaries per platform
- Distributie: GitHub Releases + (optioneel) Homebrew / winget / AUR

### Android
- Bron: `_Android/app/`
- Build: `./gradlew assembleRelease` → APK
- Distributie: GitHub Releases + icthorse.nl/quickbasic-emulator/apk/ (consistent met RandomRingtone-deploy)

### Decompiler
- Bron: `_Decompiler/src/` (Rust)
- Build: `cargo build --release` → `qbedec` binary per platform
- Distributie: GitHub Releases

## Niveau 4: Fasering (B5-Optie D hybride)

### Fase 1 (v0.0.1 — v0.6.0)
- Core in TypeScript
- Web is QBJS-fork
- X86 is QB64-PE-fork
- Android is WebView-wrapper
- Decompiler is Rust-CLI met eigen AST

### Fase 2 (v0.7.0 — v0.9.x)
- Core krijgt Rust crate-laag met WASM-bindings
- Web blijft host, gebruikt Rust-parser via WASM
- X86 gebruikt Rust-parser via FFI
- Android krijgt optioneel native-runtime-pad

### Fase 3 (v1.0+)
- Volledig unified Rust-runtime in Core
- Web/X86/Android worden dunne shells
- Forks (QBJS, QB64-PE) afgebouwd of als referentie behouden

## Niveau 5: Test-suite-discipline

Canonieke test-suite in `_Core/tests/`:
- Per dialect: tien voorbeeld-`.bas`-bestanden met expected-output
- Web en X86 moeten **alle drie dialecten** dezelfde output produceren
- Decompiler heeft eigen `_Decompiler/tests/corpus/`: `.bas` → compileer naar `.exe` (BYO-QB45) → decompile → diff met origineel

## Cross-repo relaties

| Van | Naar | Type |
|---|---|---|
| _Web | _Core | path-dep (npm workspace) |
| _X86 | _Core | spec-files via git-submodule of vendored |
| _Decompiler | _Core | AST-format vendored (Rust-types gegenereerd uit JSON-schema) |
| _Android | _Web | build-output gebundeld in `assets/web-build/` |
| Meta_QuickBasicEmulator | alle | docs-coördinatie, geen runtime-link |
| Meta_Retro_Computing | Meta_QuickBasicEmulator | sub-master verwijst project-meta aan |
| Meta_Master | Meta_Retro_Computing | master verwijst sub-master aan |
