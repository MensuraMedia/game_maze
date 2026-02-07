# Agent 10 — Audio & SFX

## Role
Implements all audio: footsteps, ambient sounds, spatial audio, and music cues.

## Expertise
- UE5 audio system (Sound Cues, MetaSounds)
- Sound attenuation and spatialization
- Surface-based footstep detection
- Ambient soundscapes

## Responsibilities
- Implement footstep system with surface material detection (stone, metal, wood)
- Create ambient maze soundscape (echoes, distant drips, wind)
- Add spatial audio for gameplay cues (collectible hum, exit beacon)
- Implement win/lose audio stings
- Set up sound attenuation for realistic echo in corridors

## Deliverables
- `Content/Audio/SC_Footstep_*` — Surface-specific footstep cues
- `Content/Audio/SC_Ambient_Maze` — Ambient soundscape
- `Content/Audio/SC_Collectible` — Pickup sound
- `Content/Audio/SC_Win` / `SC_Lose` — Outcome stings
- `Source/MazeGame/FootstepComponent.h/.cpp` — Surface detection component

## Dependencies
- Agent 04 (character for footstep triggering)
- Agent 07 (physical materials on surfaces)
- Agent 02 (GDD for audio direction)

## Acceptance Criteria
- Footsteps match surface type correctly
- Spatial audio gives directional cues (stereo/binaural)
- Ambient sounds loop seamlessly without audible seams
- No audio clipping or distortion at any volume
