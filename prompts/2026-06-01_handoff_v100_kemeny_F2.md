---
date: 2026-06-01
repo: Meta_QuickBasicEmulator
status: closed
resume: ""
---

## RESOLVED — 01-06

F2 is LIVE via commit `08ec8b7` in `QuickBasicEmulator_X86` (v0.5.0-Hopper-tag, functioneel doorgerold naar v1.0.0-Kemeny). End-to-end: Compile (Docker sandbox QB64-PE) → Run (Xvfb :100+ + binary + x11vnc + websockify in container) → noVNC iframe in browser via nginx ws-proxy `/qbe-vnc/<port>/` → Mouse/keyboard round-trip werkt. Daarna gerold naar F4 production polish (`6d5f0ed`) + F6 security hardening met gVisor (`fff4460`). MVP F1-F4 + F6 LIVE op `horsecloud55.ddns.net/basic/native/`. Alleen F5 sound resteert (post-MVP).

Repo-admin sync afgerond: _X86/version.json + Meta/version.json + ROADMAP.md + ACTIONS.md bijgewerkt naar v1.0.0-Kemeny LIVE-stand.

---

# Originele handoff (historisch)

# F2 Handoff — runtime-stream voor compiled binary

## Status F1 (COMPLEET, LIVE)

- **qbe-runner backend** :4001 op HC55 via Docker-sandbox
- **Form**: https://horsecloud55.ddns.net/basic/native/
- **POST**: https://horsecloud55.ddns.net/qbe-runner/api/compile (multipart "source")
- **Test bewezen**: K2026.BAS → 1.23MB binary in 3.7s
- **Server**: `QuickBasicEmulator_X86/server/` (server.js, Dockerfile, entrypoint.sh, qbe-runner.service)

## F2 scope

1. **Container met Xvfb + x11vnc** — uitbreiding van `qbe-compiler:latest` Dockerfile met:
   - `xvfb`, `x11vnc`, `websockify`, evt `pulseaudio` (optie, F5)
   - Entrypoint splitsen: `compile` mode (huidige) vs `run` mode (start Xvfb + binary)
2. **Backend `/api/run/<sessionId>`** — start runtime-container met binary uit eerder gecompileerde sessie
3. **Port-pool 5901-5999** voor x11vnc per sessie
4. **Frontend**: `basic/native/` toevoegen noVNC iframe + auto-redirect na compile-success
5. **noVNC client**: vendored kopie in `/opt/basic/native/novnc/` (clone novnc/noVNC repo)

## Architectuur

```
Compile OK → sessionId
  ↓
POST /api/run/<sessionId>
  ↓
Backend spawnt 2e container: Xvfb :99 + DISPLAY=:99 ./output (binary uit workdir)
  + x11vnc -display :99 -forever -nopw -listen 0.0.0.0 -rfbport 5901+sessionId%1000
  + websockify 0.0.0.0:6901+ → localhost:5901+
  ↓
Returns {vncWebsocketUrl, sessionId, expiresAt}
  ↓
Frontend iframe src = noVNC viewer + vncWebsocketUrl
```

## nginx voor websocket-proxy

```nginx
location ~ ^/qbe-vnc/(\d+)/ {
  proxy_pass http://127.0.0.1:$1/;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  proxy_read_timeout 600s;
}
```

## Veiligheid

- Container `--network none` blijft
- 5min auto-cleanup van runtime-container
- 1 sessie per IP gelijktijdig
- noVNC zonder password (per-session port volstaat als secret)

## Eerste stap volgende sessie

1. `/verifyrules` (fase 0)
2. WhatIf-bevestiging architectuur
3. Verbreed Dockerfile met Xvfb + x11vnc + websockify packages
4. Voeg `mode=run` aan entrypoint.sh toe (huidige mode = `compile`)
5. Backend `/api/run/<sessionId>` endpoint

Effort: 4-8u F2 alleen.
