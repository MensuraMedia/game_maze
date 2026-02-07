# Skill: Minimap System

## ID
`minimap`

## Description
Render a real-time top-down minimap showing maze layout, player position,
and key points of interest.

## Agents
Primary: Agent 11 (UI/UX) | Support: Agent 05 (Blueprints)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| MapSize | float | 200.0 | Widget size in pixels |
| Zoom | float | 3.0 | Zoom level (cells visible) |
| ShowFog | bool | true | Fog of war (unexplored areas hidden) |
| ShowExit | bool | true | Show exit marker |
| PlayerIcon | Texture2D | Arrow | Player icon on map |

## Steps
1. Create SceneCapture2D actor positioned above the maze, orthographic
2. Render to a RenderTarget2D at low resolution
3. Create UMG widget displaying the render target in a circular mask
4. Update capture actor position to follow player each frame
5. Optional: fog-of-war via dynamic material masking visited cells
6. Overlay icons for player, exit, collectibles

## Output
- Functional minimap widget on HUD
- Fog-of-war system (if enabled)
- Configurable zoom and style

## Performance Target
< 0.5ms GPU cost per frame
