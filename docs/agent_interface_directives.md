# Agent Interface Directives

Derived from `002_maze_guide.md`. These directives apply to **all agents**.

---

## Core Directive

All agents must interface with the local UE Editor by any means necessary to achieve
individual and collective goals. This includes programmatic, manual, and automated
methods available on the Windows 11 workstation.

## Priority Order for Editor Interaction

1. **Clipboard-Console Pipeline** — Fastest for live editor control. Agent generates
   command, copies to clipboard via PowerShell, user executes `~ → Ctrl+V → Enter → ~`.
   Use `ue_bridge.py` functions for chained operations. See `docs/clipboard_console_automation.md`.
2. **Python Editor Scripting** — First choice for complex automation (spawn actors, modify
   levels, import assets, generate procedural content). Execute via `py "path/to/script.py"`.
3. **C++ APIs** — For performance-critical or engine-level changes (custom classes,
   components, systems)
4. **Blueprints** — For rapid prototyping and gameplay logic iteration
5. **Remote Control API** — For external tool/AI integration via HTTP/WebSocket
6. **Console Commands** — For runtime debugging, NavMesh rebuilds, rendering toggles
7. **Commandlets** — For batch processing and automated testing
8. **OS-Level Input Injection** — For fully automated pipelines: Win32 SendInput,
   pyautogui, or keyboard library for direct keystroke simulation. See
   `docs/clipboard_console_automation.md` section 9.
9. **Manual with Instructions** — When direct AI control isn't feasible, provide precise
   step-by-step shortcut key sequences from `docs/agent_shortcut_reference.md`

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
