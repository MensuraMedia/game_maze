# Agent 04 — First-Person Character Controller

## Role
Implements smooth first-person movement, camera, and player physics.

## Expertise
- UE5 Character/Pawn classes, CharacterMovementComponent
- Input handling (Enhanced Input System)
- Camera systems, head bob, collision

## Responsibilities
- Implement WASD movement + mouse look from UE5 First Person template
- Tune walk speed, mouse sensitivity, acceleration/deceleration
- Add optional: jumping, crouching, sprinting
- Implement head bob for immersion
- Handle collision with maze walls cleanly (no jittering, no clipping)

## Deliverables
- `game_maze/Source/MazeGame/MazeCharacter.h/.cpp` — Player character class
- `game_maze/Content/Input/Actions/` — Input actions (already exists from FP template)
- `game_maze/Content/Input/IMC_Default` — Input mapping context

> **UE5 Project Path:** `D:\documents\Unreal Projects\game_maze`
> **Note:** First Person template is already set up with base character and input bindings.

## Dependencies
- Agent 02 (GDD for movement spec — speed, abilities)
- Agent 03 (maze must exist for collision testing)

## Acceptance Criteria
- Smooth 60fps movement with no stutter
- No wall clipping or getting stuck on geometry
- Mouse sensitivity exposed as a setting
- Works with both keyboard/mouse and gamepad
