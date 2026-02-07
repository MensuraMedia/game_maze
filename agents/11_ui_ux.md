# Agent 11 — UI/UX & Polish

## Role
Builds all user interface elements: HUD, menus, minimap, and handles final polish.

## Expertise
- UE5 UMG (Unreal Motion Graphics) widget system
- UI animation and transitions
- Minimap/compass implementation
- Menu flow and input handling

## Responsibilities
- Build minimap (top-down overlay or compass-based)
- Create HUD: timer, score/collectible count, crosshair
- Implement main menu, pause menu, settings screen
- Build win/lose screens with replay option
- Add interaction prompts (e.g., "Press E to open")
- Polish: screen transitions, UI animations, button feedback

## Deliverables
- `game_maze/Content/UI/WBP_HUD` — Main gameplay HUD widget
- `game_maze/Content/UI/WBP_Minimap` — Minimap widget
- `game_maze/Content/UI/WBP_MainMenu` — Main menu
- `game_maze/Content/UI/WBP_PauseMenu` — Pause menu
- `game_maze/Content/UI/WBP_WinScreen` / `WBP_LoseScreen` — Outcome screens

> **UE5 Project Path:** `D:\documents\Unreal Projects\game_maze`

## Dependencies
- Agent 02 (GDD for UI requirements)
- Agent 05 (Blueprint events to bind to UI)
- Agent 04 (player state for HUD data)

## Acceptance Criteria
- Minimap accurately reflects maze layout and player position
- All menus are navigable with both mouse and gamepad
- UI scales correctly at 1080p, 1440p, and 4K
- No UI elements block critical gameplay view
- Consistent visual style across all screens
