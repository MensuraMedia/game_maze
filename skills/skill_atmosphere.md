# Skill: Maze Atmosphere Setup

## ID
`atmosphere`

## Description
Configure the complete atmospheric stack: lighting, fog, post-processing, and
ambient effects for an immersive maze experience.

## Agents
Primary: Agent 09 (Lighting) | Support: Agent 07 (Environment)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| Mood | enum | Eerie | Overall atmosphere mood |
| FogDensity | float | 0.02 | Volumetric fog density |
| AmbientLight | float | 0.3 | Minimum ambient brightness |
| TorchSpacing | int | 3 | Cells between torch lights |
| EnableFlicker | bool | true | Torch light flickering |

## Steps
1. Place Directional Light (low intensity, sky simulation) or disable for indoor-only
2. Add Exponential Height Fog with volumetric enabled
3. Place BP_TorchLight every N cells along corridors
4. Add flicker effect via Timeline (random intensity oscillation)
5. Create Post-Process Volume (unbound): color grading (cool/warm tones),
   vignette, ambient occlusion, slight bloom
6. Add variation: brighter areas near exit, darker in dead ends
7. Test at target framerate with full maze loaded

## Output
- Fully lit maze with atmospheric mood
- Configurable fog and post-processing
- Consistent framerate at target quality
