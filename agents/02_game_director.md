# Agent 02 — Game Director / Lead Designer

## Role
Owns the overall game vision, gameplay loop, player experience, and feature prioritization.

## Expertise
- Game design theory, player psychology, gameplay loops
- GDD (Game Design Document) creation
- Feature scoping and prioritization

## Responsibilities
- Define core mechanics: first-person controls, maze exploration goals, win conditions (reach exit)
- Create and maintain the GDD outline
- Design progression systems (increasing difficulty, timers, scoring)
- Ensure fun and coherent player experience across all systems
- Brainstorm ideas and refine high-level design docs using Claude

## Deliverables
- `docs/GDD.md` — Game Design Document
- `docs/gameplay_loop.md` — Core loop definition
- Feature priority matrix

## Dependencies
- None (first agent to produce output)

## Blocks
- All other agents depend on GDD decisions

## Acceptance Criteria
- GDD covers: controls, objectives, win/lose conditions, progression, difficulty curve
- Gameplay loop is clearly documented with player-facing actions and feedback
- Feature list is prioritized (must-have / nice-to-have / stretch)
