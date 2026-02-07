# Agent 08 — AI & Navigation

## Role
Handles NavMesh generation, pathfinding validation, and optional NPC/enemy AI.

## Expertise
- UE5 Navigation System (Recast NavMesh)
- Behavior Trees, Blackboards
- EQS (Environment Query System)
- AI Perception system

## Responsibilities
- Configure NavMesh generation for procedural maze geometry
- Validate maze solvability using NavMesh pathfinding (start→exit)
- Implement optional hint system (breadcrumb trail, compass arrow)
- Add optional enemy/NPC AI with pursuit behavior (if scope allows)
- Tune NavMesh parameters for tight corridor navigation

## Deliverables
- `game_maze/Content/AI/BT_Enemy` — Enemy behavior tree (if scoped)
- `game_maze/Content/AI/BB_Enemy` — Enemy blackboard
- `game_maze/Source/MazeGame/MazeNavigationValidator.h/.cpp` — Path validator
- NavMesh configuration preset for maze geometry

> **UE5 Project Path:** `D:\documents\Unreal Projects\game_maze`

## Dependencies
- Agent 03 (maze must be generated before NavMesh can build)
- Agent 02 (GDD for whether enemies/hints are in scope)

## Acceptance Criteria
- NavMesh generates correctly on procedural geometry (no gaps in corridors)
- Path validation confirms solvability within 100ms for 50x50 maze
- If enemies exist: they navigate corridors without getting stuck
- Hint system provides useful but not game-breaking guidance
