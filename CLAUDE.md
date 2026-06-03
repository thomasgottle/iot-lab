# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A superproject (aggregator repo) that collects the deliverables for the TUM IoT Lab (course context `ge64hop`) as **git submodules**. It contains no code of its own — only submodule pointers and `.gitmodules`. Each submodule is a self-contained exercise with its own `CLAUDE.md`; **read the submodule's `CLAUDE.md` before working inside it**, as the toolchain and conventions differ per deliverable.

| Submodule | Deliverable | Tech | Has tests |
|---|---|---|---|
| `iotrl26d11payloads-ge64hop` | D1 Part 1 — JSON payloads & JSON Schemas | JSON only; pytest validation | yes (`pytest tests/`) |
| `iotrl26maintds250428-ge64hop` | D1 Part 2 — WoT Thing Descriptions | JSON/XML/shell; validate at [TD Playground](https://playground.thingweb.io/) | no |
| `iotrl26d21cons-ge64hop` | D2 Part 1 — WoT consumer clients | TypeScript + [node-wot](https://github.com/eclipse/thingweb.node-wot) (yarn) | no |

## Working with submodules

```bash
git submodule update --init --recursive   # populate submodules after clone
```

Commit changes **inside** each submodule first, then commit the updated pointer in this superproject. A submodule showing "modified (untracked/new commits)" in `git status` here means its checked-out commit differs from the recorded pointer.

## Known quirk

`iotrl26d21cons-ge64hop` is tracked as a submodule gitlink but is **missing from `.gitmodules`**, so `git submodule status` errors with "no submodule mapping found" for it. The other two are registered normally. If you need submodule commands to cover all three, add the missing `[submodule "iotrl26d21cons-ge64hop"]` entry to `.gitmodules`.
