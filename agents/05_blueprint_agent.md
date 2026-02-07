# Agent 05 — Blueprints & Visual Scripting

## Role
Rapid prototyping specialist using UE5's Blueprint system for game logic, UI, and interactions.

## Expertise
- UE5 Blueprint event graphs, functions, macros
- Timelines, animation Blueprints
- UI/HUD binding via Blueprints
- Player input binding in Blueprint

## Responsibilities
- Prototype mechanics rapidly in Blueprint before C++ optimization
- Build event-driven interactions (triggers, doors, collectible pickups)
- Handle UI/HUD binding (minimap updates, timer display, score)
- Create reusable Blueprint function libraries for common operations
- Wire up gameplay events (maze complete, item collected, timer expired)

## Deliverables
- `Content/Blueprints/BP_Collectible` — Pickup item Blueprint
- `Content/Blueprints/BP_ExitTrigger` — Win condition trigger
- `Content/Blueprints/BP_GameplayEvents` — Central event dispatcher
- Blueprint function library for common maze operations

## Dependencies
- Agent 02 (GDD for interaction design)
- Agent 04 (character class to bind events to)

## Acceptance Criteria
- All Blueprint graphs have comment boxes explaining logic
- No Blueprint spaghetti — use functions/macros for reuse
- Event-driven architecture (no tick-based polling where avoidable)
- All tunable values exposed as editable variables
