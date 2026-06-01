---
date: 2026-06-01
repo: Meta_QuickBasicEmulator
status: open
resume: "v1.0.0-Kemeny F1 debug: qb64pe 'UNEXPECTED INTERNAL COMPILER ERROR Line 0' op hello.bas binnen Docker"
---

# Hand-off F1 partial — qbe-runner backend werkt, qb64pe-compile faalt intern

## Vorige hand-off (start v1.0.0-Kemeny)
`Meta_QuickBasicEmulator/prompts/2026-06-01_handoff_v100_kemeny.md`

## F1 status

**80% klaar.** End-to-end wiring werkt. Resterend: één diepere QB64-PE-runtime-issue.

| Component | Status |
|---|---|
| Docker installed HC55 | ✅ 29.1.3 |
| /opt/qbe-runner/ deployed | ✅ |
| qbe-compiler:latest image | ✅ 987MB, QB64-PE compiled from source |
| qbe-runner.service | ✅ active :4001 localhost |
| GET /api/health | ✅ JSON-response |
| POST /api/compile | ✅ multipart-receive, validation, sandbox-spawn |
| Source-mount in container | ✅ `/work/input.bas` readable by qbe user |
| qb64pe compile execution | ❌ "UNEXPECTED INTERNAL COMPILER ERROR Line 0" |

## Debugging-route voor verse sessie

### Symptoom
```
$ docker run --rm --network none -v /tmp/qbe-debug:/work --user qbe:qbe qbe-compiler:latest
QB64-PE Compiler V4.5.0-UNKNOWN
Beginning C++ output from QB64 code...
UNEXPECTED INTERNAL COMPILER ERROR!
Caused by (or after):
LINE 0:
```

Tried (geen effect):
- `-c` (default compile)
- `-x` (console-only)
- `-p -x` (purge precompiled eerst)
- `-z` (transpile alleen, geen build) — NIET geprobeerd!

### Stappen volgende sessie

1. **Eerst `-z` proberen**: alleen C-code-generation, geen build. Als dit werkt: probleem zit in C-compile-stap (linker missing libs?).
2. **Direct in container debuggen**:
   ```
   ssh horsecloud55 'docker run --rm -it -v /tmp/qbe-debug:/work --user qbe:qbe --entrypoint /bin/bash qbe-compiler:latest'
   cd /opt/qb64pe
   ./qb64pe -x /work/input.bas -o /work/output  # interactief
   ```
3. **Check internal/temp/ permissions**: gebruiker qbe schrijft naar internal/temp/ tijdens compile?
4. **QB64-PE GitHub issues**: zoek op "UNEXPECTED INTERNAL COMPILER ERROR Line 0" — community-fix bestaat mogelijk
5. **TTY allocation**: misschien wil qb64pe een TTY voor stdin (zelfs met `-x`). Probeer `docker run -it ...`
6. **Image-content-check**: zijn `internal/c/qbx.o` + `libqb_make_*.o` aanwezig in stage-2 image? (Zou moeten — COPY --from=build pakt alles)

### Workaround als debug niet binnen 1u lukt

QB64-PE op HC55 native installeren ZONDER Docker (direct in /opt/qb64pe/). Backend invoke't binary direct via `child_process.spawn('/opt/qb64pe/qb64pe', ['-x', ...])`. Geen container-isolation meer maar lever werkende compile-pipeline. F2 (Xvfb/noVNC) kan later container terugbrengen.

## Code-locaties

- Backend: `QuickBasicEmulator_X86/server/server.js`
- Dockerfile: `QuickBasicEmulator_X86/server/Dockerfile`
- Entrypoint: `QuickBasicEmulator_X86/server/entrypoint.sh`
- systemd: `QuickBasicEmulator_X86/server/qbe-runner.service` → `/etc/systemd/system/qbe-runner.service` op HC55
- Workdir base: `/var/lib/qbe-runner/sessions/` (NOT /tmp — PrivateTmp issue!)
- Image tag: `qbe-compiler:latest` op HC55

## Volgende milestones (na F1 fix)

- F2 (4-8u): Xvfb + x11vnc + websockify in dezelfde container
- F3 (2-4u): Frontend HTML form + noVNC iframe in _Web/native/
- F4 (2u): Keyboard/mouse via VNC handled natively
- F5+ post-MVP
