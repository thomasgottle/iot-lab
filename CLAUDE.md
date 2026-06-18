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
| `iotrl26d3semanticrobotics-ge64hop` | D3 | node-wot robotics simulation (`simulationServer.js`) driving a CoppeliaSim scene (`TaskAssets/IoT_Remote_Lab.ttt`); levels 0–3 + device-failure | npm — `npm install`; needs **CoppeliaSim 4.10** + Docker running (see its `Setup.md`) |

Only D1 Part 1 has automated tests. The D2 submodules are TypeScript on [node-wot](https://github.com/eclipse/thingweb.node-wot) `^0.9.1`, compiled with `tsc` and run as plain Node: `node <path>/<file>.js` after a build. D3 is also node-wot `0.9.1` but ships runnable JS (`simulationServer.js`) and requires an external CoppeliaSim simulator + Docker — it cannot be exercised from this repo alone.

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

# D3 — robotics simulation (start CoppeliaSim 4.10 + `docker compose up` first; see Setup.md)
cd iotrl26d3semanticrobotics-ge64hop && npm install
npm run simulation        # or: npm run level0 | level1 | level2 | level3 | deviceFailure
```

## Working with submodules

Commit changes **inside** each submodule first, then commit the updated pointer in this superproject. A submodule showing "modified (new commits / untracked content)" in the superproject's `git status` means its checked-out commit differs from the recorded pointer **or** its working tree is dirty. `git add <submodule>` only ever stages the submodule's HEAD commit (the gitlink SHA) — it can never stage files living *inside* the submodule, so it's a no-op when the pointer already matches.

`(untracked content)` specifically is the parent recursing into the submodule and finding untracked files there — most often a leftover **Claude Code worktree** at `<submodule>/.claude/worktrees/<name>/`. Clear it inside the submodule with `git worktree remove <submodule>/.claude/worktrees/<name>` (only the submodule's own checkout is dirty; the parent has nothing to commit for it). To silence untracked noise without removing it, set `submodule.<name>.ignore = untracked` in `.gitmodules`.

The two D2.2 / D2.3 submodules are hosted on TUM Artemis (`artemis.tum.de`); **`.gitmodules` embeds personal access tokens in their clone URLs** — treat that file as secret-bearing and avoid pasting it into logs, issues, or external tools. `d11payloads` and `maintds…` instead carry **local filesystem paths** as their `url`, so a fresh `git submodule update --init` won't fetch those two from a remote — they must already exist locally (or have their URL repointed).

## Known quirk

Two submodules — `iotrl26d21cons-ge64hop` and `iotrl26d3semanticrobotics-ge64hop` — are tracked as **gitlinks** (mode `160000` in `git ls-files --stage`) but are **missing from `.gitmodules`**. So `git submodule status` / `git submodule update` error with `no submodule mapping found in .gitmodules for path '<name>'`, and they won't auto-populate on clone. The other four (`d11payloads`, `maintds…`, `d22cons`, `newmain23cons`) are registered normally. To make submodule commands cover all six, add a `[submodule "<name>"]` entry to `.gitmodules` for each.
