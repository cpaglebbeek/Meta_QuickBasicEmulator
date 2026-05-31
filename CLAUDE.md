# CLAUDE.md — Meta_QuickBasicEmulator

## Rol

Project-meta voor QuickBasicEmulator. Coördineert 5 platform-/component-repos (Core, Web, X86, Android, Decompiler).

## Sessie-startprotocol

1. Pull Meta_Master + Meta_Retro_Computing + deze repo + actieve platform-repo
2. Lees `PROJECTS.json` en `ROADMAP.md` voor actuele milestone + codenaam
3. Lees `ARCHITECTURE.md` voor cross-repo relaties bij wijzigingen die meerdere repos raken
4. Check `BUGLIST.md` op terugkerende patronen vóór nieuw werk

## Cross-repo wijzigingen

Bij wijziging die meerdere repos raakt (bv. dialect-spec-update):
1. Wijzig eerst in `_Core` (bron van waarheid)
2. Run test-suite in `_Core` — moet groen blijven
3. Propageer naar `_Web` en `_X86` — beide moeten dezelfde test-suite halen
4. Bump versie in alle geraakte repos
5. Update `ROADMAP.md` indien milestone-relevant

## Versie-bump-regel

Bij elke wijziging die naar GitHub gepushed wordt: bump minimaal `+0.0.1` in `version.json` van de geraakte repo(s) (conform `feedback_randomringtone_versioning`).

## Deploy-protocol Web

Per `feedback_icthorse_deploy`:
1. `cd _Web && npm run build`
2. rsync naar Hostinger `~/domains/icthorse.nl/public_html/quickbasic-emulator/`
3. LiteSpeed cache purge

## Code-locaties

| Wat | Waar |
|---|---|
| Dialect-spec (bron van waarheid) | `_Core/src/spec/dialect_*.json` |
| AST-types | `_Core/src/ast/types.ts` |
| Test-suite (canoniek) | `_Core/tests/` |
| Web-runtime | `_Web/src/` (QBJS-fork v0.0.3+) |
| X86-runtime | `_X86/src/` (QB64-PE-fork v0.3.0+) |
| Decompiler | `_Decompiler/src/` (Rust) |
| Android | `_Android/app/src/main/` (Kotlin/Compose) |
| Sessie-transcripties | `Meta_QuickBasicEmulator/prompts/` |

## Documentatie

- `README.md` — overzicht
- `PROJECTS.json` — machine-readable
- `ROADMAP.md` — codenamen + milestones
- `ARCHITECTURE.md` — componenten + relaties cross-repo
- `LEGAL.md` — MS-trademarks, BYO-BRUN45-policy
- `ACTIONS.md` — openstaand werk
- `BUGLIST.md` — bekende + terugkerende patronen
