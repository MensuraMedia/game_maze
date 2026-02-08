# Maze Game — UE5 First-Person 3D Maze Explorer

## Project Overview
A 3D first-person maze exploration game built in Unreal Engine 5 on Windows 11.
The player walks, explores, and navigates walls/corridors with procedural generation,
minimap, collectibles, win/lose conditions, lighting, and sound.

## Paths
- **Planning & Docs Repo:** `D:\projects\maze` (this repo — agents, tasks, skills, docs)
- **UE5 Project:** `D:\documents\Unreal Projects\game_maze` (engine project files, Content, Source)

## Architecture
- **Engine:** Unreal Engine 5 (C++ / Blueprints) — First Person template base
- **IDE:** Visual Studio 2022
- **VCS:** Git
- **AI Assistance:** Claude Code for code generation, Blueprint scripting, procedural logic

## Agent Organization
This project uses a multi-agent workflow. See `agents/` for role definitions.
- **Conductor** (`agents/00_conductor.md`) — orchestrates all agents, resolves conflicts
- **Team Lead** (`agents/01_team_lead.md`) — manages sprint tasks, tracks progress
- Specialist agents (`agents/02–11`) handle domain-specific work

## Editor Interface
All agents must interface with the UE Editor programmatically where possible.
- **Interface Reference:** `docs/UE_Interface_Reference.md` — comprehensive API/tool reference
- **Interface Directives:** `docs/agent_interface_directives.md` — mandatory agent behavior
- **Python Templates:** `docs/python_automation_templates.md` — ready-to-use automation scripts
- **Clipboard-Console Automation:** `docs/clipboard_console_automation.md` — **advanced direct-control reference** for executing generated commands, scripts, and shortcut sequences inside the UE Editor via the clipboard pipeline (`~` console → `Ctrl+V` → `Enter`). Contains 12 concrete scenarios, compound workflow patterns, error recovery, and per-agent examples. Agents should consult this when they need to verify, debug, or modify the live editor state without restarting.
- **Per-Agent Shortcuts:** `docs/agent_shortcut_reference.md` — targeted shortcut summaries per agent role
- **Console Commands:** `docs/console_commands_reference.md` — domain-organized console command catalog
- Primary methods: Python scripting, C++ APIs, Blueprints, Remote Control API, Console commands, Clipboard-Console pipeline

## Task Tracking
Task breakdowns live in `tasks/`. The Team Lead maintains the active sprint board.

## Skills
Reusable skill definitions live in `skills/`. Each skill maps to a repeatable capability.

## Conventions
- UE5 First Person template as starting base (already created at UE5 project path)
- C++ source goes in `D:\documents\Unreal Projects\game_maze\Source\`
- Content assets go in `D:\documents\Unreal Projects\game_maze\Content\`
- C++ for performance-critical systems (maze gen, navigation)
- Blueprints for rapid prototyping, UI, interaction
- Modular asset approach: wall/floor/ceiling pieces
- Iterative: compile in VS2022, hot-reload in UE Editor
- All procedural generation must guarantee solvability

## Existing UE5 Project Structure
The UE5 project already contains:
- `Content/FirstPerson/` — First Person template (Anims, Blueprints)
- `Content/Characters/Mannequins/` — Default mannequin character assets
- `Content/Input/Actions/` and `Content/Input/Touch/` — Input system
- `Content/LevelPrototyping/` — Prototype meshes, materials, interactables
- `Content/DemoTemplate/` — Sample content (Niagara, Metasounds, Sequencer, etc.)
- `Config/` — Engine and project configuration
- `game_maze.uproject` — Project file
