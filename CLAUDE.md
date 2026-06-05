# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A superproject (aggregator repo) that collects the deliverables for the TUM IoT Lab (course context `ge64hop`) as **git submodules**. It contains no code of its own — only submodule pointers and `.gitmodules`. Each submodule is a self-contained exercise; some carry their own `CLAUDE.md` (e.g. `iotrl26d21cons-ge64hop`) — **read it before working inside that submodule**, as the toolchain and conventions differ per deliverable.

| Submodule | Deliverable | What's inside | Build / validate |
|---|---|---|---|
| `iotrl26d11payloads-ge64hop` | D1 Part 1 | JSON payloads + matching JSON Schemas (`N_*.json` / `N_*.schema.json`) | `pip install -r requirements.txt && pytest tests/` |
| `iotrl26maintds250428-ge64hop` | D1 Part 2 | WoT Thing Descriptions (JSON), one XML payload, a shell auth script | validate full TDs at [TD Playground](https://playground.thingweb.io/); no automated tests |
| `iotrl26d21cons-ge64hop` | D2 Part 1 | node-wot consumer clients for a coffee machine over HTTP **and** CoAP (`HTTP/`, `CoAP/`, `Multi/`) | yarn — `yarn install && yarn build` |
| `iotrl26d22cons-ge64hop` | D2 Part 2 | node-wot consumer (`myLovelyGarden.ts`, HTTP/CoAP/file bindings) | npm — `npm install && npm run build` (`tsc`) |
| `iotrl26newmain23cons-ge64hop` | D2 Part 3 | node-wot consumers driving lab devices (Sense HAT, Hue, pan/tilt, etc.) — `N_*.ts` | npm — `npm install && npm run build` (`tsc`) |

Only D1 Part 1 has automated tests. Everything in D2 is TypeScript on [node-wot](https://github.com/eclipse/thingweb.node-wot) `^0.9.1`, compiled with `tsc` and run as plain Node: `node <path>/<file>.js` after a build.

## Common commands

```bash
git submodule update --init --recursive   # populate submodules after clone (see quirk below)

# D1 Part 1 — JSON Schema validation
cd iotrl26d11payloads-ge64hop && pip install -r requirements.txt && pytest tests/
pytest tests/test_tasks.py -k <name>      # run a single test by name

# D2 Part 1 — coffee-machine consumers (yarn)
cd iotrl26d21cons-ge64hop && yarn install && yarn build
yarn 1_checkEspresso                       # named scripts run compiled HTTP/*.js
yarn coap_1_checkEspresso                   # CoAP variants are prefixed coap_
# endpoint URLs live in url-config.json

# D2 Part 2 / Part 3 — no run scripts; build then invoke the compiled file
cd iotrl26d22cons-ge64hop && npm install && npm run build && node myLovelyGarden.js
cd iotrl26newmain23cons-ge64hop && npm install && npm run build && node 1_senseHatTempDisplay.js
```

## Working with submodules

Commit changes **inside** each submodule first, then commit the updated pointer in this superproject. A submodule showing "modified (new commits / untracked content)" in the superproject's `git status` means its checked-out commit differs from the recorded pointer.

The two D2.2 / D2.3 submodules are hosted on TUM Artemis (`artemis.tum.de`); **`.gitmodules` embeds personal access tokens in their clone URLs** — treat that file as secret-bearing and avoid pasting it into logs, issues, or external tools.

## Known quirk

`iotrl26d21cons-ge64hop` is tracked as a submodule **gitlink** (it appears in `git ls-files --stage` as mode `160000`) but is **missing from `.gitmodules`**. So `git submodule status` / `git submodule update` error with `no submodule mapping found in .gitmodules for path 'iotrl26d21cons-ge64hop'`, and that submodule won't auto-populate. The other four (`d11payloads`, `maintds…`, `d22cons`, `newmain23cons`) are registered normally. To make submodule commands cover all five, add a `[submodule "iotrl26d21cons-ge64hop"]` entry to `.gitmodules`.
