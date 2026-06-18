# IoT Lab (ge64hop)

Aggregator repository collecting my deliverables for the **TUM IoT Lab**. Each deliverable lives in its own git submodule, so this repository contains only the submodule pointers — no source of its own.

## Deliverables

| Submodule | Deliverable | Topic | Stack |
|---|---|---|---|
| [`iotrl26d11payloads-ge64hop`](iotrl26d11payloads-ge64hop) | D1 · Part 1 | JSON payloads & JSON Schemas | JSON, pytest |
| [`iotrl26maintds250428-ge64hop`](iotrl26maintds250428-ge64hop) | D1 · Part 2 | Web of Things Thing Descriptions | JSON / XML / shell |
| [`iotrl26d21cons-ge64hop`](iotrl26d21cons-ge64hop) | D2 · Part 1 | WoT consumers — coffee machine over HTTP & CoAP | TypeScript, [node-wot](https://github.com/eclipse/thingweb.node-wot) |
| [`iotrl26d22cons-ge64hop`](iotrl26d22cons-ge64hop) | D2 · Part 2 | WoT consumer — `myLovelyGarden` | TypeScript, node-wot |
| [`iotrl26newmain23cons-ge64hop`](iotrl26newmain23cons-ge64hop) | D2 · Part 3 | WoT consumers — lab devices (Sense HAT, Hue, pan/tilt, …) | TypeScript, node-wot |
| [`iotrl26d3semanticrobotics-ge64hop`](iotrl26d3semanticrobotics-ge64hop) | D3 | WoT robotics simulation — CoppeliaSim scene driven over HTTP & CoAP | Node / node-wot, CoppeliaSim, Docker |

Most submodules have their own `README` describing the specific tasks; D3 (`iotrl26d3semanticrobotics-ge64hop`) documents its environment in `Setup.md` instead.

## Getting started

Clone with submodules:

```bash
git clone --recurse-submodules <repo-url>
```

Or, if you already cloned without them:

```bash
git submodule update --init --recursive
```

> **Note:** Two submodules — `iotrl26d21cons-ge64hop` and `iotrl26d3semanticrobotics-ge64hop` — are tracked as gitlinks but are missing from `.gitmodules`, so the commands above won't populate them and `git submodule status` reports `no submodule mapping found` for those paths. Fetch them separately or add the missing `[submodule]` entries to `.gitmodules`. Additionally, `iotrl26d11payloads-ge64hop` and `iotrl26maintds250428-ge64hop` are recorded with **local filesystem paths** as their URLs, so a fresh `--recurse-submodules` clone on another machine cannot fetch those two from a remote.

## Working with submodules

Make and commit changes **inside** a submodule first, then commit the updated pointer in this superproject:

```bash
cd iotrl26d21cons-ge64hop
# ... make changes ...
git commit -am "..."        # commit inside the submodule
cd ..
git commit -am "Update iotrl26d21cons-ge64hop"   # record the new pointer
```

## Validation & tests

Tooling differs per deliverable — see each submodule's `README` / `CLAUDE.md` / `Setup.md` for details:

- **D1 Part 1** — `pip install -r requirements.txt && pytest tests/`
- **D1 Part 2** — validate full Thing Descriptions at [TD Playground](https://playground.thingweb.io/)
- **D2 Part 1** — `yarn install && yarn build`, then run a named script (e.g. `yarn 1_checkEspresso`, or its `coap_`-prefixed CoAP variant)
- **D2 Part 2 / Part 3** — `npm install && npm run build`, then `node <file>.js` (e.g. `node myLovelyGarden.js`)
- **D3** — start **CoppeliaSim 4.10** and `docker compose up` first (see the submodule's `Setup.md`), then `npm install` and `npm run simulation` (or a level script: `npm run level0` … `level3`, `npm run deviceFailure`)
