# Game Design Document — Maze Explorer

**Version:** 1.0
**Status:** Active
**Author:** Agent 02 (Game Director)
**Engine:** Unreal Engine 5 | **Platform:** PC (Windows)

---

## 1. Game Overview

**Title:** Maze Explorer (working title)
**Genre:** First-person exploration / puzzle
**Target Audience:** Casual to mid-core PC gamers, fans of exploration and spatial puzzles
**Session Length:** 2–10 minutes per maze run

**Elevator Pitch:**
Navigate a procedurally generated 3D maze from first-person view. Collect items,
beat the clock, and find the exit — every run is different.

---

## 2. Core Gameplay Loop

```
┌─────────────────────────────────────────────────────────┐
│                    CORE LOOP                            │
│                                                         │
│  START ──→ Explore ──→ Collect ──→ Navigate ──→ EXIT   │
│    │          │           │           │          │      │
│    │          ▼           ▼           ▼          │      │
│    │      Discover     +Score     Decide path   │      │
│    │      corridors    +Time      at junctions  │      │
│    │                                             │      │
│    │          ┌── Timer expires ──→ LOSE ───┐   │      │
│    │          │                              │   │      │
│    └──────────┴──── Reach exit ──→ WIN ─────┴───┘      │
│                         │                               │
│                    Stats screen                         │
│                    ┌────┴────┐                          │
│                    │  Retry  │ (new maze, same or       │
│                    │  Quit   │  harder difficulty)      │
│                    └─────────┘                          │
└─────────────────────────────────────────────────────────┘
```

**Loop Timing:**
- Exploration creates tension (unknown layout, ticking clock)
- Collectibles reward curiosity and detours
- Dead ends create frustration → relief cycle
- Exit discovery delivers the payoff

---

## 3. Player Mechanics

### 3.1 Movement

| Action | Input | Details |
|--------|-------|---------|
| Walk | WASD | Base speed 400 UU/s |
| Look | Mouse | X/Y sensitivity, pitch clamped ±89° |
| Sprint | Left Shift (hold) | 1.6x walk speed, unlimited duration |
| Crouch | Left Ctrl (toggle) | 0.5x speed, half-height capsule, access vents |
| Jump | Spacebar | Disabled by default (no vertical maze elements in MVP) |
| Interact | E | Context-sensitive (doors, switches — post-MVP) |

### 3.2 Camera

- **Height:** 170 UU (eye level for ~1.8m character)
- **FOV:** 90° default, configurable 70–110° in settings
- **Head Bob:** Subtle sinusoidal on walk (amplitude 2 UU, frequency matches step rate)
- **Sprint Shake:** Slight increase in bob amplitude + 0.5° FOV pulse
- **Collision:** Camera pulls forward to prevent clipping into walls

### 3.3 Character Capsule

- **Radius:** 34 UU
- **Half-Height:** 88 UU (standing), 44 UU (crouching)
- **Step Height:** 20 UU (smoothly step over small ledges)
- Must fit comfortably through single-cell corridors (200 UU wide)

---

## 4. Maze System

### 4.1 Generation

| Parameter | Value | Notes |
|-----------|-------|-------|
| Algorithm | Recursive Backtracking | Perfect maze (no loops), guaranteed solvable |
| Cell Size | 200 x 200 UU (floor) | Corridor width after walls subtracted |
| Wall Height | 300 UU | Taller than standing player (170 UU eye height) |
| Wall Thickness | 20 UU | Thin but solid, instanced static mesh |
| Default Size | 10 x 10 cells | Level 1 starting size |
| Max Size | 50 x 50 cells | Upper bound for performance |
| Seed | Configurable | -1 for random, fixed seed for reproducibility |

### 4.2 Layout Rules

- **Start Cell:** (0, 0) — bottom-left corner
- **Exit Cell:** (Rows-1, Cols-1) — top-right corner (maximum distance from start)
- **Solvability:** Validated post-generation via flood fill; regenerate if invalid
- **Dead Ends:** Natural result of recursive backtracking (~25% of cells in a perfect maze)
- **Open Rooms:** Post-generation pass: randomly remove 5–10% of internal walls to create junctions and open areas (breaks "perfect maze" monotony)

### 4.3 Difficulty Scaling

| Level | Grid Size | Timer | Collectibles | Open Walls Removed |
|-------|-----------|-------|-------------|-------------------|
| 1 | 8 x 8 | 120s | 3 | 10% |
| 2 | 12 x 12 | 150s | 5 | 8% |
| 3 | 16 x 16 | 180s | 8 | 6% |
| 4 | 20 x 20 | 210s | 10 | 4% |
| 5 | 25 x 25 | 240s | 13 | 3% |
| 6+ | +5 per level | +30s | +3 | -0.5% (min 1%) |

Timer is generous early to teach the loop, tighter at higher levels.

---

## 5. Collectibles

### 5.1 Standard Collectible (Gem)

- **Appearance:** Rotating gem/crystal, emissive glow, small point light
- **Placement:** Dead ends and long corridors (reward exploration)
- **Behavior:** Hover + rotate (bob amplitude 10 UU, rotation 90°/s)
- **Pickup:** Overlap trigger → collect sound → particle burst → destroy
- **Value:** 100 points each

### 5.2 Bonus Collectible (Rare Key — post-MVP)

- **Appearance:** Larger, distinct color (gold), stronger glow
- **Placement:** 1 per maze, farthest dead end from exit
- **Value:** 500 points + unlocks bonus in stats

### 5.3 Score System

| Event | Points |
|-------|--------|
| Collect gem | +100 |
| All gems collected bonus | +500 |
| Time bonus (seconds remaining × 10) | Variable |
| Level completion base | +200 × level number |

**Total Score** = Sum of all events across all levels in a session.

---

## 6. Win/Lose Conditions

### 6.1 Win

- **Trigger:** Player overlaps exit trigger volume at exit cell
- **Response:**
  1. Freeze player input
  2. Play win audio sting (triumphant, short ~2s)
  3. Fade to white (0.5s)
  4. Show Win Screen (stats + next level / quit options)

### 6.2 Lose

- **Trigger:** Timer reaches 0
- **Response:**
  1. Freeze player input
  2. Play lose audio sting (somber, short ~2s)
  3. Fade to red tint (0.5s)
  4. Show Lose Screen (progress % + retry / quit options)

### 6.3 Stats Shown on Outcome

| Stat | Description |
|------|-------------|
| Time Taken / Time Remaining | How long the player took / had left |
| Gems Collected | X / Total |
| Score | Breakdown of points earned |
| Cells Explored | % of maze visited (fog of war data) |
| Level | Current difficulty level |

---

## 7. Visual Style

### 7.1 Art Direction

- **Theme:** Ancient stone dungeon / underground labyrinth
- **Mood:** Eerie but not horror — mysterious, atmospheric, inviting exploration
- **Palette:** Cool greys and blues for stone, warm oranges for torchlight, green/cyan for collectible glow, gold for exit marker

### 7.2 Environment Materials

| Material | Application | Style |
|----------|-------------|-------|
| M_Wall_Stone | Primary walls | Rough-cut stone blocks, moss in crevices |
| M_Floor_Stone | Floor tiles | Worn flagstone, subtle cracks |
| M_Ceiling_Stone | Ceiling | Smooth stone, darker tone |
| M_Wall_Brick | Variation walls | Brick pattern, warmer tone |
| M_Floor_Metal | Variation floors | Rusted metal grate (rare sections) |

All materials use master material with instances for variation (roughness, color tint, moss amount).

### 7.3 Lighting

- **Primary:** Point lights simulating wall torches, every 3 cells
- **Color Temperature:** 2700K warm (torches), 6500K cool (ambient fill)
- **Torch Flicker:** Random intensity oscillation (Timeline, 0.8–1.2x base intensity)
- **Ambient:** Very low directional fill (0.3 intensity) — no pitch-black areas
- **Exit Glow:** Bright warm light emanating from exit cell (visible from adjacent corridors)
- **Volumetric Fog:** Low density (0.02), warm tint near torches, cool tint in dark areas

### 7.4 Post-Processing

| Effect | Setting | Purpose |
|--------|---------|---------|
| Vignette | 0.4 intensity | Focus attention, claustrophobic feel |
| Bloom | 0.6 intensity | Torch and collectible glow |
| Ambient Occlusion | Medium | Depth in corners |
| Color Grading | Slight desaturation, blue shadows | Dungeon mood |
| Exposure | Fixed manual exposure | Consistent brightness |
| Motion Blur | Off | Clean FPS movement |

---

## 8. Audio Design

### 8.1 Footsteps

| Surface | Sound Character | Trigger |
|---------|----------------|---------|
| Stone floor | Hard, echoey tap | Walk step interval (0.45s) |
| Metal grate | Metallic clang, ring | Walk step interval |
| Variable | Pitch ±10%, random from 4 variations | Per step |

Sprint footsteps: shorter interval (0.3s), louder volume (+20%).

### 8.2 Ambient

- **Base layer:** Low rumble/drone (looping, seamless)
- **Detail layers:** Distant dripping water (random interval 3–8s), faint wind whistle, occasional distant stone scrape
- **Spatialized:** Drips and echoes positioned randomly in maze, 3D attenuation

### 8.3 Gameplay Audio

| Event | Sound | Spatial |
|-------|-------|---------|
| Collectible idle | Gentle hum/chime | 3D, attracts player |
| Collectible pickup | Bright ding + sparkle | 2D (UI layer) |
| Timer warning (30s) | Ticking clock, increasing tempo | 2D |
| Timer warning (10s) | Heartbeat + rapid tick | 2D |
| Win | Triumphant fanfare (brass + chime, ~2s) | 2D |
| Lose | Descending tone + low drone (~2s) | 2D |
| Exit proximity | Warm resonant hum, louder as closer | 3D, from exit |

### 8.4 Music

- **Gameplay:** No continuous music (ambient soundscape instead) — preserves tension
- **Menu:** Soft atmospheric pad, mysterious tone
- **Win/Lose:** Short stings only (no extended tracks)

---

## 9. User Interface

### 9.1 HUD (In-Game)

```
┌───────────────────────────────────────────────────┐
│  [Timer: 1:42]                    [Minimap]       │
│                                   ┌─────────┐     │
│                                   │  ·  ▲   │     │
│                                   │ ─┼──┤   │     │
│                                   │  ·  ·   │     │
│                                   └─────────┘     │
│                                                   │
│                     +                             │
│                                                   │
│                                                   │
│  [Gems: 3/8]                    [Score: 1200]     │
└───────────────────────────────────────────────────┘
```

| Element | Position | Details |
|---------|----------|---------|
| Timer | Top-left | MM:SS, turns red at 30s, pulses at 10s |
| Minimap | Top-right | 200x200px, circular mask, fog of war |
| Crosshair | Center | Small dot/cross, white, semi-transparent |
| Gem Counter | Bottom-left | "X / Y" with gem icon |
| Score | Bottom-right | Running total, flashes on increase |

### 9.2 Minimap

- **Type:** Top-down SceneCapture2D, orthographic
- **Zoom:** Shows ~5 cell radius around player
- **Fog of War:** Unexplored areas blacked out; reveals as player walks
- **Icons:** Player arrow (rotates with view), exit marker (if discovered), gem markers
- **Style:** Circular mask with subtle border glow

### 9.3 Main Menu

```
       MAZE EXPLORER

       ▶ New Game
         Continue (if save exists)
         Settings
         Quit

       [v0.1.0]
```

### 9.4 Pause Menu

```
       PAUSED

       ▶ Resume
         Restart Maze
         Settings
         Quit to Menu
```

### 9.5 Settings

| Setting | Range | Default |
|---------|-------|---------|
| Mouse Sensitivity | 0.1–5.0 | 1.0 |
| FOV | 70–110 | 90 |
| Master Volume | 0–100 | 80 |
| SFX Volume | 0–100 | 80 |
| Music Volume | 0–100 | 60 |
| Fullscreen / Windowed | Toggle | Fullscreen |
| Resolution | System list | Native |
| VSync | On/Off | On |
| Head Bob | On/Off | On |

### 9.6 Win Screen

```
       ★ MAZE COMPLETE ★

       Time:           1:18 / 2:00
       Gems:           8 / 8  ✓ ALL COLLECTED
       Cells Explored: 87%
       Score:          +800 (gems) +500 (bonus) +420 (time) +400 (level)
       ─────────────────────
       Total:          2120

       ▶ Next Level
         Retry
         Quit to Menu
```

### 9.7 Lose Screen

```
       ✗ TIME'S UP

       Progress:       62% explored
       Gems:           5 / 8
       Score:          500

       ▶ Retry
         Quit to Menu
```

---

## 10. Technical Specifications

### 10.1 Performance Targets

| Metric | Target |
|--------|--------|
| Frame Rate | 60 fps (minimum) at 1080p |
| Maze Generation | < 1s for 50×50 |
| NavMesh Build | < 2s for 50×50 |
| Memory | < 2 GB total |
| Load Time | < 3s from menu to gameplay |

### 10.2 UE5 Systems Used

| System | Purpose |
|--------|---------|
| Enhanced Input System | All player input |
| CharacterMovementComponent | Physics-based movement |
| Instanced Static Meshes | Wall/floor rendering (draw call optimization) |
| Lumen | Dynamic global illumination |
| UMG | All UI widgets |
| Navigation System (Recast) | NavMesh for solvability validation |
| MetaSounds / Sound Cues | Audio playback |
| Post-Process Volume | Visual effects stack |

### 10.3 Module Dependencies

```
MazeGame (primary module)
├── Core, CoreUObject, Engine, InputCore
├── EnhancedInput
├── NavigationSystem
├── AIModule
├── UMG
└── Slate, SlateCore
```

---

## 11. Feature Priority Matrix

### Must-Have (MVP)

| # | Feature | Agent(s) |
|---|---------|----------|
| 1 | Procedural maze generation (recursive backtracking) | 03, 06 |
| 2 | First-person character controller (WASD + mouse) | 04 |
| 3 | Sprint mechanic | 04 |
| 4 | Win condition (reach exit) | 06, 05 |
| 5 | Timer-based lose condition | 06, 05 |
| 6 | Collectible gems with score | 05 |
| 7 | Basic HUD (timer, gem count, score) | 11 |
| 8 | Minimap with fog of war | 11 |
| 9 | Modular wall/floor assets with materials | 07 |
| 10 | Torch lighting + volumetric fog | 09 |
| 11 | Footstep audio (surface-aware) | 10 |
| 12 | Win/lose screens with stats | 11 |
| 13 | Main menu + pause menu | 11 |
| 14 | Difficulty progression (increasing maze size) | 02, 06 |
| 15 | Settings screen (sensitivity, volume, FOV) | 11 |

### Nice-to-Have (Post-MVP)

| # | Feature | Agent(s) |
|---|---------|----------|
| 16 | Crouch mechanic + vent passages | 04, 03 |
| 17 | Open room areas (wall removal pass) | 03 |
| 18 | Ambient audio layers (drips, wind) | 10 |
| 19 | Post-processing polish (bloom, vignette, AO) | 09 |
| 20 | Head bob camera effect | 04 |
| 21 | Exit proximity audio/visual cue | 10, 09 |
| 22 | Multiple material variations per zone | 07 |

### Stretch (Future)

| # | Feature | Agent(s) |
|---|---------|----------|
| 23 | Enemy AI (pursuer in maze) | 08 |
| 24 | Rare bonus collectible (key) | 05 |
| 25 | Multiplayer race mode | 06 |
| 26 | Leaderboard / high score persistence | 06, 11 |
| 27 | Theme variations (sci-fi, garden) | 07, 09 |
| 28 | Maze algorithm selector (Prim's, Kruskal's) | 03 |

---

## 12. Data Structures (Reference for C++ Agents)

### MazeCell
```
struct FMazeCell
{
    bool bNorthWall = true;
    bool bSouthWall = true;
    bool bEastWall = true;
    bool bWestWall = true;
    bool bVisited = false;       // generation flag
    bool bPlayerExplored = false; // fog of war flag
    bool bHasCollectible = false;
    bool bIsStart = false;
    bool bIsExit = false;
    int32 Row;
    int32 Col;
};
```

### MazeConfig
```
struct FMazeConfig
{
    int32 Rows = 10;
    int32 Cols = 10;
    float CellSize = 200.0f;
    float WallHeight = 300.0f;
    float WallThickness = 20.0f;
    int32 Seed = -1;           // -1 = random
    int32 NumCollectibles = 5;
    float OpenWallPercent = 0.1f; // % internal walls to remove post-gen
};
```

### GameState Data
```
enum class EMazeGameState : uint8
{
    MainMenu,
    Playing,
    Won,
    Lost,
    Paused
};

- CurrentLevel: int32
- TimeRemaining: float
- Score: int32
- GemsCollected: int32
- TotalGems: int32
- CellsExplored: int32
- TotalCells: int32
- GameState: EMazeGameState
```

---

*This document is the binding reference for all agents. Changes require Game Director approval.*
*See `docs/gameplay_loop.md` for the detailed loop breakdown.*
