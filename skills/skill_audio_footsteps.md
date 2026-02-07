# Skill: Surface-Aware Footsteps

## ID
`footsteps`

## Description
Detect surface material under player and play matching footstep sounds
with spatial audio and variation.

## Agents
Primary: Agent 10 (Audio) | Support: Agent 04 (Character Controller)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| Surfaces | array | [Stone, Metal, Wood] | Supported surface types |
| VariationsPerSurface | int | 4 | Sound variations to avoid repetition |
| StepInterval | float | 0.45 | Seconds between footsteps at walk speed |
| VolumeScale | float | 0.7 | Footstep volume |

## Steps
1. Create Physical Materials for each surface type in UE
2. Assign Physical Materials to floor material instances
3. Create FootstepComponent (ActorComponent on Character)
4. On movement: line-trace downward to detect Physical Material
5. Select Sound Cue from matching surface set (randomized variation)
6. Play at foot location with attenuation (spatial)
7. Scale interval by movement speed (faster walk = shorter interval)

## Output
- Surface-aware footstep playback
- No repetitive "machine gun" effect (randomized selection + pitch variation)
- Correct spatial positioning
