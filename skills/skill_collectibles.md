# Skill: Collectible System

## ID
`collectibles`

## Description
Place and manage collectible items throughout the maze with pickup interaction,
scoring, and visual/audio feedback.

## Agents
Primary: Agent 05 (Blueprints) | Support: Agent 10 (Audio), Agent 09 (Lighting)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| Count | int | 10 | Number of collectibles per maze |
| SpawnRule | enum | DeadEnds | Placement strategy |
| PointValue | int | 100 | Score per pickup |
| RotationSpeed | float | 90.0 | Degrees/second spin |
| BobAmplitude | float | 10.0 | Hover bob height |

## Steps
1. After maze generation, identify placement cells (dead ends, long corridors)
2. Spawn BP_Collectible actors at selected cells (centered, hovering)
3. Add idle animation: rotation + vertical bob via Timeline
4. Add point light with matching color for visibility
5. On overlap with player: play pickup sound, add score, destroy actor
6. Update HUD collectible counter
7. Optional: track "all collected" for bonus condition

## Output
- Collectible actors placed in maze
- Score integration with GameState
- Visual + audio feedback on pickup
