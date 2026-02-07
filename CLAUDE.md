# Maze Game — UE5 First-Person 3D Maze Explorer

## Project Overview
A 3D first-person maze exploration game built in Unreal Engine 5 on Windows 11.
The player walks, explores, and navigates walls/corridors with procedural generation,
minimap, collectibles, win/lose conditions, lighting, and sound.

## Architecture
- **Engine:** Unreal Engine 5 (C++ / Blueprints)
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
- UE5 First Person template as starting base
- C++ for performance-critical systems (maze gen, navigation)
- Blueprints for rapid prototyping, UI, interaction
- Modular asset approach: wall/floor/ceiling pieces
- Iterative: compile in VS2022, hot-reload in UE Editor
- All procedural generation must guarantee solvability
