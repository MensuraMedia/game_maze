# Maze Game — UE5 First-Person 3D Maze Explorer

A 3D first-person maze exploration game built in Unreal Engine 5 on Windows 11, developed using an AI-driven multi-agent workflow powered by Claude.

## Game Concept

The player navigates a procedurally generated 3D maze from a first-person perspective. Explore corridors, collect items, avoid dead ends, and find the exit before time runs out. Features include dynamic lighting, spatial audio, a minimap, and increasing difficulty.

## Project Structure

### Planning Repo (`D:\projects\maze`)
```
maze/
├── agents/                    # Agent role definitions (12 agents)
│   ├── 00_conductor.md        # Top-level orchestrator
│   ├── 01_team_lead.md        # Technical lead & task manager
│   ├── 02–11_*.md             # Specialist agents (see Agent Workflow)
│   └── agent_dependency_graph.md
├── tasks/                     # Sprint planning & tracking
│   ├── sprint_board.md        # Active sprint task board
│   └── decisions_log.md       # Technical decisions record
├── skills/                    # Reusable skill definitions (7 skills)
├── docs/                      # Design documents & references
├── CLAUDE.md                  # Claude Code project instructions
├── README.md                  # This file
└── 001_maze_guide.md          # Original development guide
```

### UE5 Project (`D:\documents\Unreal Projects\game_maze`)
```
game_maze/
├── game_maze.uproject         # Unreal project file
├── Config/                    # Engine & project config
├── Content/
│   ├── FirstPerson/           # FP template (Anims, Blueprints)
│   ├── Characters/Mannequins/ # Mannequin assets, materials, textures
│   ├── Input/Actions/         # Enhanced Input actions
│   ├── LevelPrototyping/      # Prototype meshes, interactables
│   ├── DemoTemplate/          # Samples (Niagara, Metasounds, etc.)
│   └── ...                    # New game content goes here
├── Source/                    # C++ source (to be created)
├── Intermediate/              # Build intermediates (not in VCS)
├── DerivedDataCache/          # Cached data (not in VCS)
└── Saved/                     # Local saves (not in VCS)
```

## Agent Workflow

This project uses 12 specialized AI agents organized in a hierarchy:

| # | Agent | Role |
|---|-------|------|
| 00 | **Conductor** | Orchestrates all agents, resolves conflicts, manages integration |
| 01 | **Team Lead** | Breaks down tasks, reviews code, makes technical decisions |
| 02 | **Game Director** | Defines game vision, mechanics, and player experience |
| 03 | **Level Designer** | Procedural maze generation & spatial design |
| 04 | **Character Controller** | First-person movement, camera, collision |
| 05 | **Blueprint Agent** | Rapid prototyping, interactions, event systems |
| 06 | **C++ Programmer** | Performance-critical code, engine classes, optimization |
| 07 | **Environment Artist** | Modular 3D assets, materials, visual style |
| 08 | **AI & Navigation** | NavMesh, pathfinding validation, optional NPC AI |
| 09 | **Lighting & Atmosphere** | Lights, fog, post-processing, mood |
| 10 | **Audio & SFX** | Footsteps, ambient sound, spatial audio, music |
| 11 | **UI/UX & Polish** | HUD, minimap, menus, interaction prompts |

## Development Phases

1. **Foundation** — GDD, UE5 project setup, maze generation, character controller, base assets
2. **Gameplay Loop** — Collectibles, win/lose conditions, NavMesh, footsteps
3. **UI & Polish** — HUD, minimap, menus, ambient audio, post-processing
4. **Integration & QA** — Full build, playtesting, bug fixes, optimization, release

## Tech Stack

- **Engine:** Unreal Engine 5.x
- **Language:** C++ (systems) + Blueprints (gameplay/UI)
- **IDE:** Visual Studio 2022
- **Platform:** Windows 11
- **AI Tooling:** Claude (code generation, design assistance, debugging)
- **Version Control:** Git

## File Locations

| What | Path |
|------|------|
| **This repo** (planning, agents, tasks, skills, docs) | `D:\projects\maze` |
| **UE5 project** (engine files, Content, Source, Configs) | `D:\documents\Unreal Projects\game_maze` |
| **UE5 project file** | `D:\documents\Unreal Projects\game_maze\game_maze.uproject` |

The UE5 project was created from the **First Person** template and already includes:
- First Person character Blueprints and animations (`Content/FirstPerson/`)
- Mannequin character assets (`Content/Characters/Mannequins/`)
- Enhanced Input System actions (`Content/Input/Actions/`)
- Level prototyping meshes and materials (`Content/LevelPrototyping/`)
- Demo template content including Niagara, Metasounds, and Sequencer samples

## Skills (Reusable Capabilities)

| Skill | Description |
|-------|-------------|
| `maze-gen` | Procedural maze generation with solvability guarantee |
| `fps-controller` | First-person movement with Enhanced Input System |
| `minimap` | Real-time top-down minimap with fog of war |
| `collectibles` | Item placement, pickup interaction, scoring |
| `win-lose` | Game state management, timers, outcome screens |
| `atmosphere` | Lighting, fog, post-processing stack |
| `footsteps` | Surface-aware spatial footstep audio |

## Getting Started

1. Clone this repository to `D:\projects\maze`
2. Open the UE5 project at `D:\documents\Unreal Projects\game_maze\game_maze.uproject`
3. Build C++ via Visual Studio 2022 (generate project files from .uproject if needed)
4. Play in editor to verify First Person template works
5. Refer to `agents/` for role-specific development guidance
6. Track progress in `tasks/sprint_board.md`

## License

TBD
