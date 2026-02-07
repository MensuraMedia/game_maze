# Agent Interface Directives

Derived from `002_maze_guide.md`. These directives apply to **all agents**.

---

## Core Directive

All agents must interface with the local UE Editor by any means necessary to achieve
individual and collective goals. This includes programmatic, manual, and automated
methods available on the Windows 11 workstation.

## Priority Order for Editor Interaction

1. **Python Editor Scripting** — First choice for automation (spawn actors, modify levels,
   import assets, generate procedural content)
2. **C++ APIs** — For performance-critical or engine-level changes (custom classes,
   components, systems)
3. **Blueprints** — For rapid prototyping and gameplay logic iteration
4. **Remote Control API** — For external tool/AI integration via HTTP/WebSocket
5. **Console Commands** — For runtime debugging, NavMesh rebuilds, rendering toggles
6. **Commandlets** — For batch processing and automated testing
7. **Manual with Instructions** — When direct AI control isn't feasible, provide precise
   step-by-step instructions for the user

## Interface Discovery Protocol

When an agent needs a feature or API:

1. **Check reference:** Consult `docs/UE_Interface_Reference.md` first
2. **Console discovery:** Run `dumpconsolecommands` or `help <command>`
3. **Python introspection:** `dir(unreal)`, `help(unreal.EditorLevelLibrary)`
4. **Code search:** Search https://grep.app/ for UE-related open-source examples
5. **Claude reasoning:** Prompt for API suggestions based on UE version
6. **Create if missing:** If a required API doesn't exist, generate it (Python module,
   C++ class, or Blueprint function library) and document in the reference file

## Agent Output Requirements

Each agent must, when producing work:

1. **Provide executable code** — Python scripts, C++ source, or Blueprint instructions
   that can be directly run in the UE Editor
2. **Include fallback steps** — If direct automation fails, provide manual instructions
3. **Update the reference** — Add any newly discovered APIs/methods to
   `docs/UE_Interface_Reference.md`
4. **Test via automation** — Use UE Automation Framework or Python verification scripts

## Team Lead Responsibilities (Additional)

The Team Lead must:
- Instruct agents to query UE docs, project files, and console commands to identify features
- Guide agents to enable required plugins and install dependencies
- Assign agents to populate `docs/UE_Interface_Reference.md` by expertise area
- Escalate complex integration needs to collaborative multi-agent reasoning

## Resource Discovery

Agents may draw on:
- **grep.app** (https://grep.app/) — Search open-source UE code for snippets and examples
- **MCP integrations** — Use Model Context Protocol servers for AI-UE bridging
- **Local UE documentation** — Help > Documentation within the editor
- **UE source code** — If available via Epic Games GitHub access
- **Community plugins** — UnrealMCP, Cactus AI Framework, Ludus AI, Aura

## Automation Architecture

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│ Claude Agent │────→│ Python Scripts   │────→│ UE Editor       │
│ (reasoning)  │     │ (automation)     │     │ (execution)     │
└─────────────┘     └──────────────────┘     └─────────────────┘
       │                     │                        │
       │              ┌──────┴──────┐          ┌──────┴──────┐
       │              │ File System │          │ Remote Ctrl │
       │              │ (configs,   │          │ API (HTTP)  │
       │              │  scripts)   │          │             │
       │              └─────────────┘          └─────────────┘
       │
       └──→ Console Commands (via Python subprocess or AHK)
```
