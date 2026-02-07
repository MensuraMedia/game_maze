# Gameplay Loop — Detailed Breakdown

**Reference:** `docs/GDD.md` Section 2
**Author:** Agent 02 (Game Director)

---

## Session Flow

```
LAUNCH ──→ MAIN MENU ──→ NEW GAME ──→ LEVEL LOOP ──→ SESSION END
                │                         ▲    │
                └── CONTINUE ─────────────┘    │
                                               ▼
                                          QUIT TO MENU
```

## Level Loop (Per Maze)

### Phase 1: Initialization (0–1 seconds, no player input)
1. **Generate Maze** — MazeGenerator creates grid from FMazeConfig
2. **Validate** — Flood fill confirms path from start to exit
3. **Spawn Geometry** — Instanced static meshes for walls, floors, ceilings
4. **Place Collectibles** — Gems in dead ends and corridors per placement rules
5. **Build NavMesh** — Runtime NavMesh for solvability confirmation (debug)
6. **Spawn Player** — At start cell, facing down the first corridor
7. **Initialize HUD** — Timer set, gem counter at 0/N, minimap active
8. **Fade In** — 0.5s fade from black → gameplay begins

### Phase 2: Exploration (bulk of play time)

**Player Actions:**
| Action | System Response | Feedback |
|--------|----------------|----------|
| Move through corridors | Update position, reveal fog of war | Footstep audio, minimap updates |
| Enter new cell | Mark cell as explored | Fog of war reveals on minimap |
| Approach collectible | Collectible glow intensifies | Audio hum gets louder (3D spatial) |
| Overlap collectible | Collect → add score → destroy | Ding sound, particle burst, HUD flash |
| Reach dead end | Nothing mechanical | Tension: wasted time, must backtrack |
| Approach exit | Exit glow visible from adjacent cells | Warm audio hum, increased light |
| Sprint | 1.6x speed | Faster footsteps, slight FOV pulse |

**Timer Pressure:**
| Time Remaining | Feedback |
|----------------|----------|
| > 30s | Normal HUD display |
| 30s | Timer turns red, ticking clock audio starts |
| 10s | Timer pulses, heartbeat audio layered in |
| 0s | Timer hits zero → Lose state |

### Phase 3: Resolution

**Win Path:**
```
Player overlaps exit trigger
  → Freeze input
  → Win audio sting (2s)
  → Fade to white (0.5s)
  → Calculate score:
      Gems × 100
    + All gems bonus (500 if all collected)
    + Time remaining × 10
    + Level base (200 × level)
  → Show Win Screen with stats
  → Player choice: Next Level / Retry / Quit
```

**Lose Path:**
```
Timer reaches 0
  → Freeze input
  → Lose audio sting (2s)
  → Red tint overlay (0.5s)
  → Calculate partial score:
      Gems × 100
      (no time bonus, no level bonus)
  → Show Lose Screen with progress %
  → Player choice: Retry / Quit
```

### Phase 4: Transition

| Choice | Result |
|--------|--------|
| **Next Level** | Increment level, increase difficulty per table, generate new maze → Phase 1 |
| **Retry** | Same level settings, new seed, generate new maze → Phase 1 |
| **Quit to Menu** | Save session score if applicable → Main Menu |

---

## Moment-to-Moment Feel

### First 10 Seconds
- Player spawns, sees corridor ahead, torch-lit walls
- Minimap shows only immediate surroundings (fog of war)
- Timer visible, counting down — establishes urgency
- First footstep echoes — sets audio atmosphere

### Mid-Run (30–60% through timer)
- Player has explored several corridors, found 1–2 gems
- Minimap reveals branching paths, some dead ends
- Decisions: which fork to take, backtrack or push forward
- Collectible hum draws player toward optional detours

### Late Run (last 30 seconds)
- Timer turns red, ticking intensifies
- Player must decide: hunt remaining gems or rush exit
- If exit not found: panic exploration, rapid turns
- If exit found: relief, sprint to finish

### Reward Moment
- Reaching exit: bright glow, triumphant sound, score breakdown
- Seeing "ALL COLLECTED" bonus: satisfying for completionists
- Score increase displayed piece by piece for emphasis

---

## Player Motivations

| Motivation | How the Game Serves It |
|------------|----------------------|
| **Exploration** | Unknown maze layout, fog of war reveal, varied corridors |
| **Mastery** | Faster times, better scores, higher difficulty levels |
| **Completion** | Collect all gems, explore 100% of cells |
| **Risk/Reward** | Detour for gems vs. save time for exit |

---

## Difficulty Curve

```
Tension
  ▲
  │          ╱╲    ╱╲    ╱╲    ╱╲
  │        ╱    ╲╱    ╲╱    ╲╱    ╲  ← dead ends + timer pressure
  │      ╱                          ╲
  │    ╱  ← early exploration         ╲ ← exit found or time up
  │  ╱                                  ╲
  │╱                                      ╲
  └──────────────────────────────────────────→ Time
  Start                                    End
```

Peaks occur at dead ends (frustration) and junction decisions (uncertainty).
Valleys occur after finding collectibles (reward) and recognizing paths (progress).
Overall tension rises as timer decreases.

---

*This document supplements GDD Section 2. All agents should reference this for
understanding player experience at each moment.*
