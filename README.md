# IoT Lab (ge64hop)

Aggregator repository collecting my deliverables for the **TUM IoT Lab**. Each deliverable lives in its own git submodule, so this repository contains only the submodule pointers — no source of its own.

## Deliverables

| Submodule | Deliverable | Topic | Stack |
|---|---|---|---|
| [`iotrl26d11payloads-ge64hop`](iotrl26d11payloads-ge64hop) | D1 · Part 1 | JSON payloads & JSON Schemas | JSON, pytest |
| [`iotrl26maintds250428-ge64hop`](iotrl26maintds250428-ge64hop) | D1 · Part 2 | Web of Things Thing Descriptions | JSON / XML / shell |
| [`iotrl26d21cons-ge64hop`](iotrl26d21cons-ge64hop) | D2 · Part 1 | WoT consumer clients | TypeScript, [node-wot](https://github.com/eclipse/thingweb.node-wot) |

Each submodule has its own `README` describing the specific tasks.

## Getting started

Clone with submodules:

```bash
git clone --recurse-submodules <repo-url>
```

Or, if you already cloned without them:

```bash
git submodule update --init --recursive
```

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

Tooling differs per deliverable — see each submodule's `README` / `CLAUDE.md` for details:

- **D1 Part 1** — `pip install -r requirements.txt && pytest tests/`
- **D1 Part 2** — validate full Thing Descriptions at [TD Playground](https://playground.thingweb.io/)
- **D2 Part 1** — `yarn install && yarn build`
