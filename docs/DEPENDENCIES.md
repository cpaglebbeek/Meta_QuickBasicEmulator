# QuickBasicEmulator — Dependencies & Oorzaak-Gevolg-Matrix

Per component: wat consumeert het, wat consumeert hem, en wat is de impact bij wijziging.

---

## 1. Component-graaf

```
                    QuickBasicEmulator_Core
                    (TS dialect-spec + AST)
                      │  │  │
       ┌──────────────┘  │  └────────────┐
       │                 │               │
       ▼                 ▼               ▼
  ┌─────────┐      ┌──────────┐    ┌───────────┐
  │ _Web    │      │ _X86     │    │ _Decompiler│
  │(QBJS-fork)     │(QB64-PE-fork)  │ (Rust)    │
  └────┬────┘      └──────────┘    └───────────┘
       │
       ▼
  ┌─────────┐
  │ _Android│
  │(WebView)│
  └─────────┘
```

---

## 2. Wat consumeert wat

| Component | Consumeert | Type-relatie | Wijze |
|---|---|---|---|
| `_Web` | `_Core` | npm path-dep | `package.json` workspace |
| `_Web` | (v0.0.3+) `boxgaming/qbjs` fork | git-vendored | `src/qbjs-fork/` directory |
| `_X86` | `_Core` (dialect-spec JSON) | git-vendored | `src/spec-mirror/` of submodule |
| `_X86` | (v0.3.0+) `QB64-Phoenix-Edition/QB64pe` fork | git-vendored | `src/qb64pe-fork/` directory |
| `_Android` | `_Web` (built artifacts) | bundle-assets | `app/src/main/assets/web-build/` na `_Web npm run build` |
| `_Decompiler` | `_Core` (AST-types) | type-vendoring | Rust struct-types gegenereerd uit Core JSON-schema |
| `Meta_QuickBasicEmulator` | alle 5 platform-repos | docs-link only | Geen runtime-dep, alleen verwijzingen |
| `Meta_Retro_Computing` | `Meta_QuickBasicEmulator` | docs-link only | PROJECTS.json verwijst project-meta |
| `Meta_Master` | `Meta_Retro_Computing` | docs-link only | PROJECTS.json + ECOSYSTEMS.md verwijst sub-master |

---

## 3. Externe afhankelijkheden (upstream + tooling)

| Component | Externe dep | Licentie | Type | Wanneer |
|---|---|---|---|---|
| `_Core` | Node.js 20 | MIT (Node) | runtime | vanaf v0.0.1 |
| `_Core` | TypeScript ^5.4 | Apache-2.0 | dev | vanaf v0.0.1 |
| `_Core` | Vitest ^1.6 | MIT | dev | vanaf v0.0.1 |
| `_Web` | Vite ^5.2 | MIT | dev | vanaf v0.0.1 |
| `_Web` | `boxgaming/qbjs` | (TBD bij fork) | git-vendored | vanaf v0.0.3-Davidoff |
| `_X86` | C++17 toolchain (GCC/Clang/MSVC) | div. | compile-tool | vanaf v0.0.1 |
| `_X86` | CMake ≥3.20 | BSD-3 | build-tool | vanaf v0.0.1 |
| `_X86` | `QB64-Phoenix-Edition/QB64pe` | MIT | git-vendored | vanaf v0.3.0-Chen |
| `_Android` | Android SDK 34 (compile) / 26 (min) | proprietary toolchain | compile-tool | vanaf v0.0.1 |
| `_Android` | Kotlin 1.9.23 | Apache-2.0 | compile-tool | vanaf v0.0.1 |
| `_Android` | Jetpack Compose BOM 2024.05.00 | Apache-2.0 | runtime | vanaf v0.0.1 |
| `_Decompiler` | Rust stable (1.75+) | MIT/Apache-2.0 | compile-tool | vanaf v0.0.1 |
| `_Decompiler` | clap ^4.5 | MIT/Apache-2.0 | runtime | vanaf v0.0.1 |
| `_Decompiler` | serde ^1.0 | MIT/Apache-2.0 | runtime | vanaf v0.0.1 |
| `_Decompiler` | (v0.4.0+) object/goblin | MIT/Apache-2.0 | runtime | vanaf Stand-alone-mode |

**Spec-referenties (niet code-deps):**
- `microsoft/GW-BASIC` — MIT — spec-referentie, geen verbatim port
- `maurom/qbasic-reversing-notes` — TBD — kennisbasis voor signature-DB-bouw

---

## 4. Oorzaak-gevolg-matrix

Per shared asset / interface: wat triggert wijziging in welke andere components.

### Als `_Core/src/spec/dialect_gwbasic.json` wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_Web` | Runtime moet nieuw statement/feature ondersteunen | Test-suite groen houden; dialect-mode-parser uitbreiden |
| `_X86` | QB64-PE-fork moet patch krijgen voor GW-BASIC-mode | Patch in `patches/` + rebuild |
| `_Decompiler` | (geen direct effect — decompiler werkt op QB45) | Geen actie nodig |
| Documentatie | `ARCHITECTURE.md` mogelijk verouderd | Check sectie "Niveau 5: Test-suite-discipline" |
| Versie | Spec-wijziging is breaking voor consumers | Bump minor (v0.X.Y → v0.X+1.0) |

### Als `_Core/src/spec/dialect_qbasic.json` of `dialect_qb45.json` wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_Web` | QBJS-fork dialect-mode QB/QB45 update | Patch + test-suite groen |
| `_X86` | QB64-PE-fork dialect-flag update | Patch + test-suite groen |
| `_Decompiler` | QB45-decompilatie-output kan veranderen | Test-corpus diff-rapport verifiëren |
| Versie | Bump minor in `_Core` | Consumers moeten lock-version updaten |

### Als `_Core/src/ast/types.ts` wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_Web` | Type-import in src/main.ts kan breken | TS-build moet groen |
| `_X86` | Geen direct effect (X86 gebruikt JSON-spec, niet TS-types) | Geen actie |
| `_Decompiler` | Rust struct-types gegenereerd uit JSON-schema — schema mogelijk te updaten | Rust-codegen herrunnen |
| Versie | AST-wijziging = breaking → bump minor | Consumers updaten lock |

### Als `_Web` build-output wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_Android` | `assets/web-build/` is stale | Rerun `_Web npm run build` + kopieer naar Android-assets + bump Android version |
| icthorse.nl-deploy | Bestaande deploy stale | Deploy-pipeline triggeren (rsync + cache purge) |
| Documentatie | Geen direct effect | — |

### Als `_Decompiler` AST-output-formaat wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_Core` | Geen — Core is producer, niet consumer van decompiler-output | — |
| (toekomstige tooling, bv. round-trip-test) | Consumer-code updaten | Test-corpus diff-spec update |
| Versie | Bump Decompiler minor | RELEASES.md update |

### Als `Meta_QuickBasicEmulator/LEGAL.md` wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| Alle 6 project-repos | NOTICE.md mogelijk verouderd | Audit + sync NOTICE.md per repo |
| Decompiler watermark-template | Mogelijk te updaten | Code-template-string updaten |
| Versie | Bump Meta_QuickBasicEmulator minor (legal-policy = breaking) | Communicate naar contributors |

### Als upstream `boxgaming/qbjs` wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_Web` (vanaf v0.0.3+) | Fork is achter — overweeg rebase | NOTICE.md commit-hash updaten + manual rebase |
| Test-suite | Mogelijke nieuwe edge-cases | Test-corpus uitbreiden |

### Als upstream `QB64-Phoenix-Edition/QB64pe` wijzigt → ...

| Geraakt component | Impact | Verplichte actie |
|---|---|---|
| `_X86` (vanaf v0.3.0+) | Fork is achter | Patches in `patches/` rebasen + NOTICE.md update |

---

## 5. Cross-cutting: gedeelde infrastructuur

Geen Hetzner-shared-infra in v0.0.1 skeleton. Vanaf v0.1.0-Letwin:

| Resource | Shared met | Conflict-risico | Mitigatie |
|---|---|---|---|
| `icthorse.nl/quickbasic-emulator/` (path) | Andere icthorse.nl-paden | LiteSpeed cache shared | Cache purge per `feedback_icthorse_deploy` |
| Geen Hetzner-poort (Web is statisch) | n.v.t. | — | — |

Bij elke deploy-wijziging: lees `Meta_Master/SHARED_INFRASTRUCTURE.md`.

---

## 6. Decompiler signature-DB (BYO-lokaal, niet shared)

| Resource | Locatie | Niet gedeeld want | Wel verplicht |
|---|---|---|---|
| `BRUN45.EXE` / `BCOM45.LIB` | `~/.qbedec/qb45/` op gebruikers-machine | MS-IP, P-QBE-04 | Gebruiker bouwt lokale signature-DB |
| `~/.qbedec/signatures.json` | Lokaal | Machine-specifieke build-hash | Niet in git, niet gedeeld tussen gebruikers |

---

## 7. Wanneer deze matrix updaten

- Bij toevoegen van een nieuw component (bv. `_Wasm` fase-2)
- Bij toevoegen van een nieuwe externe dep
- Bij wijziging van een interface (spec, AST, build-output)
- Bij elke fork-import (NOTICE.md + DEPENDENCIES.md tabel rij toevoegen)
- Verplicht onderdeel van pre-release-checklist: "DEPENDENCIES.md actueel?"
