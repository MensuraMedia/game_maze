# Maze Game — UE5 First-Person 3D Maze Explorer

A 3D first-person maze exploration game built in Unreal Engine 5 on Windows 11, developed using an AI-driven multi-agent workflow powered by Claude.

## Game Concept

The player navigates a procedurally generated 3D maze from a first-person perspective. Explore corridors, collect items, avoid dead ends, and find the exit before time runs out. Features include dynamic lighting, spatial audio, a minimap, and increasing difficulty.

## Project Structure

```
maze/
├── agents/                    # Agent role definitions (AI-driven workflow)
│   ├── 00_conductor.md        # Top-level orchestrator
│   ├── 01_team_lead.md        # Technical lead & task manager
│   ├── 02_game_director.md    # Game vision & design
│   ├── 03_level_designer.md   # Procedural maze generation
│   ├── 04_character_controller.md  # FPS movement & camera
│   ├── 05_blueprint_agent.md  # Visual scripting & prototyping
│   ├── 06_cpp_programmer.md   # C++ systems & performance
│   ├── 07_environment_artist.md    # 3D assets & materials
│   ├── 08_ai_navigation.md    # NavMesh & pathfinding
│   ├── 09_lighting_atmosphere.md   # Lighting, fog, post-process
│   ├── 10_audio_sfx.md        # Sound design & spatial audio
│   ├── 11_ui_ux.md            # UI widgets, menus, minimap
│   └── agent_dependency_graph.md   # Agent execution order
├── tasks/                     # Sprint planning & tracking
│   ├── sprint_board.md        # Active sprint task board
│   └── decisions_log.md       # Technical decisions record
├── skills/                    # Reusable skill definitions
│   ├── skill_procedural_maze_gen.md  # Maze generation skill
│   ├── skill_fps_controller.md       # Character controller skill
│   ├── skill_minimap.md              # Minimap system skill
│   ├── skill_collectibles.md         # Collectible system skill
│   ├── skill_win_lose.md             # Win/lose conditions skill
│   ├── skill_atmosphere.md           # Atmosphere setup skill
│   └── skill_audio_footsteps.md      # Footstep audio skill
├── docs/                      # Design documents & references
├── Source/MazeGame/           # C++ source code
├── Content/                   # UE5 content assets
│   ├── Blueprints/            # Blueprint assets
│   ├── Meshes/                # Static meshes (walls, floors, etc.)
│   ├── Materials/             # Materials & material instances
│   ├── UI/                    # UMG widget Blueprints
│   ├── Audio/                 # Sound cues & audio assets
│   ├── Lighting/              # Light Blueprint actors
│   ├── PostProcess/           # Post-processing profiles
│   ├── Input/                 # Input actions & mapping contexts
│   └── AI/                    # Behavior trees & blackboards
├── Config/                    # UE5 config files (DefaultEngine.ini, etc.)
├── Scripts/                   # Build & automation scripts
├── Tests/                     # Test plans & automated tests
├── CLAUDE.md                  # Claude Code project instructions
└── 001_maze_guide.md          # Original development guide
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

1. Clone this repository
2. Open the UE5 project (or create from First Person template and import Source/Content)
3. Build C++ in Visual Studio 2022
4. Open the UE Editor and play in editor
5. Refer to `agents/` for role-specific development guidance
6. Track progress in `tasks/sprint_board.md`

## License

TBD
