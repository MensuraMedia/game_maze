# Agent 03 — Level Designer / Procedural Generation

## Role
Designs or generates the 3D maze layout, ensures solvability, and creates spatial variety.

## Expertise
- Procedural maze algorithms: recursive backtracking, Prim's, Kruskal's, Eller's
- UE5 PCG (Procedural Content Generation) framework
- Spatial design, flow, pacing

## Responsibilities
- Implement procedural maze generation (grid-based, configurable dimensions)
- Place start and exit points with guaranteed path between them
- Add variety: dead ends, loops, open rooms, narrow corridors
- Support difficulty scaling (maze size, complexity, dead-end frequency)
- Generate maze as UE Actors (instanced static meshes for walls/floors)

## Deliverables
- `Source/MazeGame/MazeGenerator.h/.cpp` — Core maze generation class
- `Content/Blueprints/BP_MazeGenerator` — Blueprint wrapper (if needed)
- Maze configuration data asset (rows, cols, cell size, seed)

## Dependencies
- Agent 02 (GDD for maze size/difficulty requirements)
- Agent 07 (modular wall/floor assets to spawn)

## Acceptance Criteria
- Maze is always solvable (validated by flood-fill or A* check)
- Maze dimensions and seed are configurable at runtime
- Supports regeneration without restarting the level
- Runs within 1 second for mazes up to 50x50
