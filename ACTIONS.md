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

## v0.2.0-Weiland — AFGEROND 01-06

- [x] Classic-to-structured transformer (`_Web/src/transform/classic-to-structured.ts`): GOSUB-label blocks → SUB-procedures (01-06, 13 tests, GOSUB-only, mixed-GOTO-skip)
- [x] runtime_capability_qbjs.json matrix in `_Core/spec/`: 38 full + 7 partial + 11 none, met workaround per gap (01-06, 24 tests)
- [ ] Decompiler v0.2.0-Weiland: BRUN-mode stabiel — echte p-code parser i.p.v. heuristic (uitgesteld naar v0.2.1 of v0.3.0)

## v0.3.0-Chen → v0.3.10-Chen Web — AFGEROND 01-06

- [x] Web v0.3.1: transformer pass-2 forward-GOTO→EXIT + LABEL_RE indented-labels-fix (49/49 tests)
- [x] Web v0.3.2-v0.3.10: 9 transformer-passes voor K2026 DOS-BASIC compound-syntax (pass-3a t/m pass-3c: strip-`:`-tussen-THEN-en-ELSE, split single-line WHILE/WEND, etc.)
- [x] X86 v0.3.0-Chen LIVE: QB64-PE submodule (722b7d99 MIT) + wrapper CLI + K2026C native binary 1.4MB

## v1.0.0-Kemeny — AFGEROND 01-06 — LIVE op `horsecloud55.ddns.net/basic/native/`

- [x] **F1** native compile-as-a-service backend (`_X86/server/` qbe-runner.service :4001 + Docker `qbe-compiler:latest` image) — commits `f0afe61` → `33d37e5` → `0e3732c`
- [x] **F2** runtime-stream Xvfb + x11vnc + websockify in container + nginx ws-proxy `/qbe-vnc/<port>/` + mouse/keyboard round-trip — commit `08ec8b7`
- [x] **F3** frontend-form `/basic/native/` met Compile/Run/Stop + noVNC iframe — commit `08ec8b7` (frontend onderdeel)
- [x] **F4** production polish: UX error-handling (429/400/413/network) + loading-spinner + rate-limit 20/h per IP + dangerous-keyword filter (SYSTEM/CHAIN/FILES/RUN "/_OS/_OPENCLIENT/_CONNECT/_HTTPS) + sample-dropdown (Hello/INPUT/SCREEN/FOR/COLOR) + auto-stop beforeunload — commit `6d5f0ed`
- [x] **F6** security hardening: gVisor runsc voor compile-containers + cap-drop=ALL + no-new-privileges + audit-log /var/log/qbe-runner/audit.log met source-sha256 + health-endpoint sandbox-section — commit `fff4460`
- [ ] **F5** sound (post-MVP, pulseaudio in runtime-container)

## v1.0.0-Kemeny F4 verificatie 01-06 — follow-ups

Live-curl-verificatie 01-06 bevestigde server-side F4-features (forbidden-keyword SYSTEM/_OPENCLIENT → 400 met JSON, valid compile → 200 + sessionId + binSize, health-endpoint sandbox-section + rate-limit-config). Twee verbeter-punten geconstateerd, geen F4-bug:

- [ ] **Multer file-size limit** in `_X86/server/server.js` — 1MB body bufferde tot 30s timeout i.p.v. vroege 413. Toevoegen: `multer({ limits: { fileSize: 256_000 } })` of vergelijkbaar. Niet kritisch (gVisor + cap-drop dekt resource-misbruik) maar nice-to-have hardening tegen DoS via grote multipart.
- [ ] **`X-RateLimit-*` response-headers** in `_X86/server/server.js` — momenteel alleen via 429-response-body zichtbaar. Standaard `X-RateLimit-Limit` / `X-RateLimit-Remaining` / `X-RateLimit-Reset` headers in elke `/api/compile`-response zou clients pre-emptief laten weten waar ze staan (i.p.v. blind doorgaan tot 429).
- [ ] **HTTP/2 multipart 502-onderzoek** — `curl --http2 -F` geeft 502, `--http1.1` werkt. Browsers werken wel (eigen HTTP/2-stream-framing). Mogelijk nginx-proxy-buffer-config kwestie. Niet urgent (productie werkt) maar verdient root-cause-check als andere clients (CLI-tools, CI) ooit komen.
- [ ] **Visuele UI-tests v1.0.0-Kemeny F4** — spinner-animatie tijdens compile, sample-dropdown UX, noVNC-iframe rendering, mouse/keyboard round-trip in VNC. Vereist browser-screenshot (curl-only verificatie dekte alleen API + HTML-source).

## Volgende: v0.4.0-Chien — Decompiler Stand-alone EXE mode

- [ ] BCOM45 signature-DB opbouwen (BYO-policy per P-QBE-04, geen MS-binaries in repo)
- [ ] PE-import-table parser uitbreiden (huidige BRUN-detector dekt alleen wrapped binaries)
- [ ] Functie-fingerprinting algoritme (signature-hash → AST-fragment)
- [ ] CLI flag `--mode=standalone` toevoegen + watermark per P-QBE-05
- [ ] Test-fixtures: synthetic stand-alone EXE-samples (geen echte QB45-output)

## Repo-admin sync — AFGEROND 01-06

- [x] _X86/version.json bump v0.3.0-Chen → v1.0.0-Kemeny met F1-F6 history
- [x] Meta/version.json bump v0.3.0-Chen → v1.0.0-Kemeny met live_demos
- [x] ROADMAP.md v1.0.0-Kemeny row REDESIGN → LIVE
- [x] F2-handoff `prompts/2026-06-01_handoff_v100_kemeny_F2.md` status `open` → `closed`

## Sanitycheck follow-ups (uit v0.0.1-Gates sessie)

- [x] P1-1 CI workflows actief in 5 platform-repos (01-06)
- [x] P2-1 docs/PRINCIPLES.md aangemaakt (01-06)
- [x] P2-2 docs/DEPENDENCIES.md aangemaakt (01-06)
- [ ] P3-1 docs/screens/ aanmaken bij v0.0.3-Davidoff (Web krijgt UI)
- [ ] P3-2 DESIGN_TOKENS.md aanmaken bij v0.0.3-Davidoff
- [ ] P3-3 CONTENT_INVENTORY.md aanmaken bij v0.0.3-Davidoff
- [ ] P3-4 CHANGELOG.md per repo aanmaken bij v0.1.0-Letwin
