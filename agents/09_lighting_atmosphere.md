# Agent 09 — Lighting & Atmosphere

## Role
Creates mood and immersion through lighting, fog, and post-processing.

## Expertise
- UE5 Lumen (dynamic global illumination)
- Volumetric fog, exponential height fog
- Post-processing volumes (bloom, color grading, vignette)
- Point/spot/rect lights, light channels

## Responsibilities
- Design lighting scheme: moody, tension-building maze atmosphere
- Place dynamic lights (torches, overhead glow, flickering effects)
- Configure volumetric fog for depth and mystery
- Set up post-processing volume (color grading, bloom, vignette, ambient occlusion)
- Create light variation zones (well-lit areas vs dark corridors)

## Deliverables
- `Content/Lighting/BP_TorchLight` — Reusable torch light Blueprint
- `Content/PostProcess/PP_MazeAtmosphere` — Post-process profile
- Lighting preset configurations for different maze zones
- Fog density and color settings

## Dependencies
- Agent 07 (environment assets must exist to light)
- Agent 03 (maze layout for light placement logic)
- Agent 02 (GDD for mood/tone direction)

## Acceptance Criteria
- Consistent 60fps with lighting enabled (Lumen or baked fallback)
- Atmospheric fog visible but doesn't obscure gameplay
- No pitch-black dead zones (minimum ambient light everywhere)
- Post-processing enhances mood without causing eye strain
