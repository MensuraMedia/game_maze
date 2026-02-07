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
