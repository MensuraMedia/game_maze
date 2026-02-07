# Skill: First-Person Controller Setup

## ID
`fps-controller`

## Description
Configure a smooth first-person character controller with WASD movement,
mouse look, and maze-appropriate collision handling.

## Agents
Primary: Agent 04 (Character Controller) | Support: Agent 05 (Blueprints)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| WalkSpeed | float | 600.0 | Walking speed (UU/s) |
| SprintMultiplier | float | 1.5 | Sprint speed multiplier |
| MouseSensitivity | float | 1.0 | Look sensitivity |
| EnableJump | bool | false | Allow jumping |
| EnableCrouch | bool | false | Allow crouching |
| HeadBob | bool | true | Camera head bob |

## Steps
1. Extend ACharacter with custom MazeCharacter class
2. Set up Enhanced Input: IA_Move, IA_Look, IA_Sprint, IA_Jump, IA_Crouch
3. Configure CharacterMovementComponent (speed, acceleration, friction)
4. Implement mouse look with pitch clamping (-89 to 89 degrees)
5. Add head bob via camera timeline (subtle sinusoidal on walk)
6. Set capsule collision sized for maze corridors
7. Bind input mapping context in BeginPlay

## Output
- Playable first-person character in maze
- Configurable movement parameters exposed to editor

## Quality Checks
- No wall clipping at any speed
- Smooth deceleration on stop
- No camera jitter during collision
