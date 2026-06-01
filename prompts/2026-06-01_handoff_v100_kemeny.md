---
date: 2026-06-01
repo: Meta_QuickBasicEmulator
status: open
resume: "v1.0.0-Kemeny: native compiler-as-a-service op HC55 (Docker + Xvfb + noVNC) — F1-F4 MVP"
---

# Hand-off — v1.0.0-Kemeny native compiler-as-a-service

## Aanleiding

Code-Rood-verdict einde sessie 2026-06-01: QBJS-token-patch-loop (v0.3.7-v0.3.10) hit fundamenteel-incompatibele zone. K2026 is legacy DOS-BASIC met multi-statement-per-line + DOS-specifieke patronen. QBJS is structured-QBasic-transpiler, niet DOS-emulator.

Redesign: gebruik **echte native compiler** (QB64-PE, draait Linux) op HC55, expose via HTML form + noVNC. Browser krijgt VNC-stream van Linux GUI-app, kan keyboard/mouse-events terugsturen.

## Architectuur

```
Browser                  HC55 (Linux)
─────────                ─────────────────────────────────────
HTML form ── HTTPS ────→ Backend (Node/Python) :4001
[Source upload]                 │
                                ├──→ Docker container per sessie:
noVNC client ◄─ WSS ──────────── │      ├──→ Xvfb :99 (virtual display)
[Canvas viewer]                  │      ├──→ QB64-PE compile .bas
                                 │      ├──→ Run binary onder Xvfb
Keyboard/mouse ─ WSS ─────────── │      ├──→ x11vnc + websockify
                                 │      └──→ (post-MVP: PulseAudio + WebRTC)
                                 └──→ 5min auto-cleanup, 256MB cap, no-net
```

## Beslissingen (akkoord 2026-06-01)

- **Codenaam:** v1.0.0-Kemeny (BASIC's vader, majestueuze release)
- **MVP-scope:** F1-F4 (backend + Xvfb/noVNC + frontend + keyboard/mouse) — geen sound
- **Sandbox:** Docker (patroon-consistent met andere HC55-services)
- **Sessie-strategie:** verse sessie (deze afsluiten, fresh starten voor v1.0.0)

## F1-F4 detail-scope

### F1 — Backend skeleton (4-8u)
- `/opt/qbe-runner/` systemd service op HC55
- Poort: kies vrije in 4000-range (4001 gepland, check SHARED_INFRASTRUCTURE.md vóór reservering)
- Endpoint: POST `/api/compile` met multipart .bas-upload (max 1MB)
- Source-validation pre-flight: reject SHELL/KILL/POKE/SYSTEM (dangerous syscalls)
- Spawn Docker container per sessie, returns `{sessionId, expiresAt, vncWebsocketPath}`
- Rate-limit per IP (10/uur)
- Logs naar `/var/log/qbe-runner/`

### F2 — Xvfb + x11vnc + noVNC (4-8u)
- Docker image: `qbe-runner-sandbox:latest`
- Base: Debian-slim + QB64-PE submodule + Xvfb + x11vnc + websockify
- Entrypoint script: start Xvfb :99, compile .bas, run binary, expose x11vnc op port 5900+(sessionId%1000)
- websockify bridge: `wss://horsecloud55.ddns.net/qbe-vnc/<sessionId>` → container's x11vnc
- nginx location block voor `/qbe-vnc/` met dynamic upstream-routing (lua of subprocess-spawn-then-proxy)

### F3 — Frontend integratie (2-4u)
- Nieuwe pagina `icthorse.nl/quickbasic-emulator/native/` (en HC55 `/basic/native/`)
- HTML form: file upload OR textarea + Submit
- Bij submit: POST naar HC55 backend, ontvang sessionId
- Embed `<iframe>` met noVNC viewer naar wss-URL
- Status-banner: session-id, expires-at countdown, "Stop session" button
- Behoud bestaande Web-UI als "quick preview" — native als 2e tab

### F4 — Keyboard/mouse (2u)
- noVNC handles keyboard/mouse natively via VNC-protocol
- Browser focus management: click iframe → events flow
- Hotkey "Esc to leave" + back-to-form

## Niet in MVP (F5-F7, post-MVP)

- F5 Sound (PulseAudio + WebRTC) — significant complexity
- F6 Security hardening (gVisor, seccomp profiles, capabilities-drop)
- F7 Multi-tenant load balancing (port-pooling, queue, fairness)

## Vereiste setup vóór F1 start

1. Docker installed op HC55: `ssh horsecloud55 'docker --version'` — als niet aanwezig, install
2. Vrije poort kiezen in 4000-range, claim in `Meta_Master/SHARED_INFRASTRUCTURE.md`
3. nginx location block voor `/qbe-runner/` proxy_pass + `/qbe-vnc/<sessionId>/` websocket-upgrade
4. ufw/iptables: backend poort + VNC-pool port-range open (intern only, niet publiek)

## Repos die werk krijgen

- **QuickBasicEmulator_X86**: voegt `server/` directory toe met backend service + Dockerfile (X86 was tot nu CLI-only)
- **QuickBasicEmulator_Web**: voegt `native/` directory toe met form-UI + noVNC iframe
- **Meta_QuickBasicEmulator**: ROADMAP-update v0.5.0-v1.0.0, ARCHITECTURE.md sectie "Server-side compile mode"

## Context bij start nieuwe sessie

Pull alle 3 repos. Lees:
- `Meta_QuickBasicEmulator/ROADMAP.md` — codenamen
- `Meta_QuickBasicEmulator/docs/PRINCIPLES.md` — P-QBE-01..10
- `Meta_QuickBasicEmulator/docs/DEPENDENCIES.md` — oorzaak-gevolg
- `Meta_Master/SHARED_INFRASTRUCTURE.md` — patronen Archi Desktop noVNC (6090/5999), poort-reservering
- `vendor/qb64pe/` submodule in _X86 — al geconfigureerd

## Tests bij start

- 117 totaal groen huidig (Core 52 + Web 50 + Decompiler 16)
- v1.0.0 voegt server-side compile-tests toe (curl-tests in CI)

## Status quo bij hand-off

- Web v0.3.10-Chen LIVE op icthorse.nl/quickbasic-emulator/ + horsecloud55.ddns.net/basic/
- X86 native v0.3.0-Chen werkt lokaal (K2026 draait in 1.4MB binary)
- Web heeft 4 transformer-passes + 3 runtime-mode buttons + .bas loader
- Code-Rood verdict gedocumenteerd in BUGLIST: QBJS-RUNTIME-GAP-DEEP

## Eerste-stap suggestie

In verse sessie:
1. Lees deze hand-off
2. /verifyrules (fase 0)
3. WhatIf-bevestiging dat architectuur klopt
4. SHARED_INFRASTRUCTURE check (poort + Docker-status HC55)
5. F1 start: skeleton backend service
