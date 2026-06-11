# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project

Single-page, zero-dependency web UI for controlling Philips Hue lights on the
local network. The entire application is one file: `index.html` (inline CSS
and vanilla JS, no build step, no framework, no external requests other than
to the Hue bridge).

- Bridge IP is configurable (pair screen and settings panel), stored in
  `localStorage`, default `192.168.2.2` (`DEFAULT_BRIDGE_IP`). New addresses
  are verified against the unauthenticated `/api/config` endpoint.
- Talks to the Hue bridge **v1 REST API** (`http://<bridge>/api/...`).
- Designed to be opened directly from `file://` — recent bridge firmware
  sends CORS headers. Fallback: serve with `python3 -m http.server`.

## Hard constraints

- **Stay a single file.** No external JS/CSS, no CDN, no npm, no build step.
- Keep vanilla JS (`"use strict"`, no transpilation needed by Firefox/Chrome).
- All persistent client state lives in `localStorage`:
  - `hue-bridge-ip` — bridge IP address
  - `hue-app-key` — bridge API key
  - `hue-room-order` — JSON array of room ids (display order)
  - `hue-light-order` — JSON map of room id → array of light ids
  - `hue-visible-types` — JSON `{rooms, zones}` booleans (settings checkboxes)
- Names of rooms/lights are stored **on the bridge** (rename uses
  `PUT /lights/<id>` and `PUT /groups/<id>`), not locally.

## Architecture (index.html)

- `api(method, path, body)` — fetch wrapper, prefixes `/api/<appKey>`.
- Pairing: `POST /api {devicetype}` after link button; manual key entry as
  alternative; key revocation (error type 1) drops back to the pair screen.
- `refresh()` polls `/lights`, `/groups`, `/sensors` every 5 s
  (`REFRESH_MS`). Paused while `interacting > 0` (slider drags) or in
  reorder/edit mode.
- `roomList()` builds display order: bridge groups of type `Room`, plus a
  synthetic `__other` room (`OTHER_ROOM_ID`) for unassigned lights. Saved
  order first, then alphabetical for unknown ids.
- `render()` rebuilds the DOM; `updateLightCard()` patches in place when the
  set of lights is unchanged. `cards` / `roomToggles` maps hold live elements.
- Reorder/edit mode (⇅): up/down arrows + ✏ rename, normal controls hidden.
- Units: Hue API uses hue 0–65535, sat 0–254, bri 1–254, ct in **mireds**
  153–500. The UI shows kelvin (K = 1e6 / mired, 100 K steps). Sensor
  temperature is 0.01 °C; light level is `10000·log10(lux)+1`.
- `PRESETS` — white recipes (Rest…Cool bright), ordered warm → cool; applied
  per light or per room (group action).
- Group `0` is the bridge-wide "all lights" group (master switch).

## Conventions

- Commit messages: Conventional Commits (`feat:`, `fix:`, …), body explains
  the why and the bridge API used.
- Log every user prompt verbatim in `PROMPT_LOG.md` (append a numbered
  section per prompt batch).
- Update `README.md` when user-visible features change.

## Testing

No automated tests. Verify against the real bridge:

```bash
curl -s http://192.168.2.2/api/config   # bridge alive, no auth needed
```

Then open `index.html` in a browser and exercise the UI.
