# Per-Agent UE5 Editor Shortcut Reference

Targeted shortcut summaries for each maze game agent, derived from the comprehensive
UE5.7 shortcut reference (`shortcut_keys_reference.md`). Each section contains only
the shortcuts relevant to that agent's domain and workflow.

> **Full Reference:** `shortcut_keys_reference.md` (400+ shortcuts across 14 categories)
> **Customization:** Edit > Editor Preferences > Keyboard Shortcuts

---

## Agent 00 — Conductor (Orchestrator)

The Conductor oversees all workflows. Needs integration testing, play-testing, and
status verification shortcuts.

### Play, Simulate & Debug
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play In Editor (PIE) — primary integration test |
| `Alt + S` | Simulate (observe without possessing) |
| `F8` | Eject/Possess player during PIE |
| `Pause` | Pause simulation |
| `Esc` | Stop PIE |
| `F11` | Immersive mode (fullscreen viewport) |
| `Ctrl + Shift + H` | Toggle FPS counter |
| `Shift + L` | Toggle stat overlays |

### Global Save & Level Management
| Shortcut | Action |
|----------|--------|
| `Ctrl + S` | Save current level |
| `Ctrl + Shift + S` | Save all |
| `Ctrl + O` | Open level |
| `Ctrl + N` | New level |

### Console (Integration Checks)
| Command | Purpose |
|---------|---------|
| `~` (tilde) | Open console |
| `stat fps` | Verify framerate during integration |
| `stat unit` | Frame time breakdown |
| `stat memory` | Memory usage check |
| `obj list` | List all UObjects (leak detection) |

---

## Agent 01 — Team Lead (Task Manager)

The Team Lead reviews code quality, manages builds, and verifies compilation.

### Build & Compilation
| Shortcut | Action |
|----------|--------|
| `Ctrl + Alt + F11` | Hot Reload C++ (Live Coding) |
| `F7` | Compile Blueprint |
| `Ctrl + S` | Save (triggers auto-compile check) |
| `Ctrl + Shift + S` | Save All |

### Code Navigation & Review
| Shortcut | Action |
|----------|--------|
| `Ctrl + P` | Quick search (find any asset/class) |
| `Ctrl + B` | Browse to asset in Content Browser |
| `Ctrl + E` | Edit selected asset |
| `Ctrl + Shift + F` | Find in All Blueprints |
| `Alt + Shift + R` | Reference Viewer (dependency check) |
| `Alt + Shift + A` | Audit Assets |
| `Alt + Shift + M` | Size Map (disk usage) |

### Play Testing
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play (PIE) |
| `Esc` | Stop PIE |
| `F8` | Eject/Possess |
| `Ctrl + Shift + H` | Toggle FPS |

---

## Agent 02 — Game Director (Lead Designer)

The Game Director focuses on play-testing, game feel, and iterating on design.

### Play & Iterate
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play In Editor |
| `Alt + S` | Simulate (observe gameplay systems) |
| `F8` | Eject to fly-cam during play |
| `Esc` | Stop play session |
| `Pause` | Pause to inspect state |

### Console (Design Tuning)
| Command | Purpose |
|---------|---------|
| `slomo 0.5` | Slow-motion (study player movement) |
| `slomo 2.0` | Speed up (skip to late-game) |
| `god` | Invincibility (focus on level flow) |
| `ghost` | No-clip (explore maze layout) |
| `fly` | Free flight mode |
| `teleport` | Teleport to crosshair point |
| `restartlevel` | Quick restart for iteration |

### View Modes
| Shortcut | Action |
|----------|--------|
| `G` | Toggle Game View (hide editor widgets) |
| `F11` | Immersive mode |
| `Alt + 4` | Lit view (final look) |

---

## Agent 03 — Level Designer (Procedural Generation)

Level Designer needs actor placement, grid snapping, and spatial verification tools.

### Transformation & Placement
| Shortcut | Action |
|----------|--------|
| `W` | Translate mode |
| `E` | Rotate mode |
| `R` | Scale mode |
| `Q` | Select mode |
| `Space` | Cycle Transform tools |
| `Alt + Drag` | Duplicate + move actor |
| `Ctrl + `` ` | Toggle World/Local coordinates |

### Snapping & Grid
| Shortcut | Action |
|----------|--------|
| `End` | Snap selected to floor |
| `Alt + End` | Snap pivot to floor |
| `Shift + End` | Snap bounds to floor |
| `Ctrl + End` | Snap to grid |
| `[` | Decrease grid snap size |
| `]` | Increase grid snap size |
| `V` | Vertex snapping (hold + drag) |

### Selection & Visibility
| Shortcut | Action |
|----------|--------|
| `Ctrl + A` | Select all |
| `Ctrl + Shift + A` | Select all of same class/mesh |
| `Shift + E` | Select matching actors |
| `H` | Hide selected (declutter viewport) |
| `Ctrl + H` | Unhide all |

### Camera & Viewport
| Shortcut | Action |
|----------|--------|
| `Alt + G` | Perspective view |
| `Alt + J` | Top-down view (verify maze layout) |
| `Alt + H` | Front view |
| `Alt + K` | Left/side view |
| `Home` | Zoom to fit selection |
| `Ctrl + 0-9` | Set camera bookmark |
| `0-9` | Jump to bookmark |
| `Alt + 2` | Wireframe (see through walls) |

### Console (Level Verification)
| Command | Purpose |
|---------|---------|
| `show collision` | Verify collision volumes |
| `show bounds` | See actor bounding boxes |
| `show navigation` | Check NavMesh coverage |
| `RebuildNavigation` | Rebuild NavMesh after changes |

---

## Agent 04 — Character Controller

Character Controller needs play-testing, camera views, and debug overlays.

### Play & Debug
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play (test movement) |
| `Esc` | Stop PIE |
| `F8` | Eject to inspect from outside |
| `Pause` | Freeze mid-movement |
| `F9` | Take screenshot |

### Console (Character Debug)
| Command | Purpose |
|---------|---------|
| `show collision` | See character capsule collision |
| `show bounds` | Character bounds visualization |
| `stat fps` | Verify 60fps movement |
| `stat unit` | Frame time (detect movement stutter) |
| `ghost` | No-clip (test wall collision) |
| `fly` | Free flight (compare to grounded) |
| `slomo 0.25` | Ultra slow-mo (debug collision) |

### View Modes (Collision Debugging)
| Shortcut | Action |
|----------|--------|
| `Alt + 2` | Wireframe (see collision shapes) |
| `Alt + 4` | Lit (final gameplay view) |
| `G` | Game view (test HUD visibility) |
| `Ctrl + Shift + H` | FPS counter (performance check) |

---

## Agent 05 — Blueprint Agent

Blueprint Agent needs the Blueprint Editor shortcuts and rapid iteration tools.

### Blueprint Editor (Primary Workspace)
| Shortcut | Action |
|----------|--------|
| `F7` | Compile Blueprint |
| `Ctrl + S` | Save Blueprint |
| `Ctrl + Z / Y` | Undo / Redo |
| `Ctrl + F` | Find in this Blueprint |
| `Ctrl + Shift + F` | Find in All Blueprints |
| `Home` | Zoom to fit graph |
| `RMB Drag` | Pan graph |
| `Mouse Wheel` | Zoom graph |
| `Ctrl + Mouse Wheel` | Fast zoom |
| `Page Up / Dn` | Navigate parent/child graphs |

### Blueprint Node Creation (Key + Click)
| Shortcut | Action |
|----------|--------|
| `B + Click` | Branch node |
| `S + Click` | Sequence node |
| `D + Click` | Delay node |
| `F + Click` | ForEach Loop |
| `C + Click` | Comment box |

### Blueprint Debugging
| Shortcut | Action |
|----------|--------|
| `F9` | Toggle breakpoint on node |
| `Ctrl + Shift + F9` | Clear all breakpoints |
| `Alt + Click (pin)` | Break link connection |

### Asset Navigation
| Shortcut | Action |
|----------|--------|
| `Ctrl + B` | Browse to asset in Content Browser |
| `Ctrl + E` | Open/edit selected asset |
| `Ctrl + P` | Quick search project |
| `Space` | Preview asset (Content Browser) |

---

## Agent 06 — C++ Systems Programmer

C++ Programmer needs build tools, debugging, and performance profiling shortcuts.

### Build & Hot Reload
| Shortcut | Action |
|----------|--------|
| `Ctrl + Alt + F11` | Hot Reload / Live Coding |
| `F7` | Compile (in Blueprint context) |

### Debugging & Profiling
| Shortcut | Action |
|----------|--------|
| `Ctrl + Shift + H` | Toggle FPS counter |
| `Shift + L` | Toggle stats overlay |
| `Ctrl + Shift + ,` | GPU Profiler |
| `Alt + P` | Play (test C++ changes) |
| `Esc` | Stop PIE |

### Console (Performance Profiling)
| Command | Purpose |
|---------|---------|
| `stat fps` | Framerate |
| `stat unit` | Frame time breakdown |
| `stat memory` | Memory usage |
| `stat scenerendering` | Rendering stats |
| `profilegpu` | GPU profiler |
| `obj list` | UObject count (leak detection) |
| `dumpconsolecommands` | List all available commands |

### Content Navigation
| Shortcut | Action |
|----------|--------|
| `Ctrl + P` | Quick search (find classes) |
| `Ctrl + B` | Browse to asset |
| `Alt + Shift + R` | Reference Viewer (dependency graph) |

---

## Agent 07 — Environment Artist

Environment Artist needs asset management, material editing, and visual inspection tools.

### Content Browser (Primary Workspace)
| Shortcut | Action |
|----------|--------|
| `Ctrl + P` | Search project assets |
| `Ctrl + B` | Browse to asset |
| `Ctrl + E` | Edit selected asset |
| `Space` | Preview asset |
| `Enter` | Open asset/folder |
| `Ctrl + Shift + N` | New folder |
| `Alt + Shift + A` | Audit Assets |
| `Alt + Shift + M` | Size Map |

### Material / Mesh Editor Viewport
| Shortcut | Action |
|----------|--------|
| `I` | Toggle environment preview |
| `O` | Toggle floor |
| `P` | Toggle post-process |
| `Alt + N` | Toggle normals display |
| `Alt + S` | Toggle sockets |
| `Ctrl + N` | Nanite fallback (Static Mesh) |

### Viewport Visual Inspection
| Shortcut | Action |
|----------|--------|
| `Alt + 2` | Wireframe (check topology) |
| `Alt + 3` | Unlit (check UV/texture only) |
| `Alt + 4` | Lit (final lighting on assets) |
| `Alt + 8` | Shader Complexity (optimization) |
| `Alt + 0` | Lightmap Density |

### Transformation (Asset Placement)
| Shortcut | Action |
|----------|--------|
| `W / E / R` | Translate / Rotate / Scale |
| `Alt + Drag` | Duplicate actor |
| `End` | Snap to floor |
| `V` | Vertex snapping |
| `[` / `]` | Grid size adjust |

### Modeling Mode (`Shift + 5`)
| Shortcut | Action |
|----------|--------|
| `Enter` | Accept/Complete operation |
| `Esc` | Cancel operation |
| `A` | Toggle gizmo |
| `E` | Extrude |
| `Q` | Push/Pull |
| `[ / ]` | Brush size +/- |
| `Ctrl + [ / ]` | Brush strength +/- |
| `T` | Flip selection |

---

## Agent 08 — AI & Navigation

AI/Navigation agent needs NavMesh visualization, simulation, and pathfinding debug tools.

### Navigation Console Commands (Primary Tools)
| Command | Purpose |
|---------|---------|
| `RebuildNavigation` | Full NavMesh rebuild after maze generation |
| `show navigation` | Toggle NavMesh overlay visualization |
| `ai.debug` | Toggle AI debugging overlays |
| `stat navigation` | NavMesh performance stats |

### Play & Simulate
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play (test navigation at runtime) |
| `Alt + S` | Simulate (observe AI without possessing) |
| `F8` | Eject to observe AI behavior |
| `Esc` | Stop |
| `Pause` | Freeze to inspect NavMesh state |

### View Modes (NavMesh Inspection)
| Shortcut | Action |
|----------|--------|
| `Alt + 2` | Wireframe (see through walls to NavMesh) |
| `Alt + 4` | Lit (normal view) |
| `G` | Game view |
| `Alt + J` | Top-down view (NavMesh coverage) |

### Console (AI Tuning)
| Command | Purpose |
|---------|---------|
| `slomo 0.5` | Slow-mo (observe AI pathing) |
| `ghost` | No-clip (inspect NavMesh from inside) |
| `show collision` | Verify collision affecting NavMesh |

---

## Agent 09 — Lighting & Atmosphere

Lighting agent needs view modes, rendering commands, and visual quality tools.

### View Modes (Critical for Lighting Work)
| Shortcut | Action |
|----------|--------|
| `Alt + 4` | Lit (see final lighting result) |
| `Alt + 3` | Unlit (isolate geometry from lighting) |
| `Alt + 5` | Detail Lighting (lighting detail only) |
| `Alt + 6` | Lighting Only (pure light contribution) |
| `Alt + 7` | Light Complexity (performance check) |
| `Alt + 8` | Shader Complexity |
| `Alt + 0` | Lightmap Density |

### Lighting Build
| Shortcut | Action |
|----------|--------|
| `Ctrl + Alt + L` | Rebuild lighting |
| Build Menu | Full rebuild (geometry + lighting + nav) |

### Console (Rendering & Atmosphere)
| Command | Purpose |
|---------|---------|
| `r.Lumen.Enabled 1/0` | Toggle Lumen GI |
| `r.VolumetricFog 1/0` | Toggle volumetric fog |
| `show fog` | Toggle fog visibility |
| `show postprocess` | Toggle post-processing |
| `show lighting` | Toggle all lighting |
| `show directionallight` | Toggle directional light |
| `show pointlight` | Toggle point lights |
| `show spotlight` | Toggle spot lights |
| `HighResShot 4` | 4x resolution screenshot |
| `stat scenerendering` | Rendering performance |
| `profilegpu` | GPU profiler for lighting cost |

### Placement & Camera
| Shortcut | Action |
|----------|--------|
| `W / E / R` | Translate / Rotate / Scale lights |
| `Alt + Drag` | Duplicate light actor |
| `Home` | Focus on selected light |
| `Ctrl + 0-9` | Bookmark lighting setups |
| `G` | Game view (see final result) |
| `F11` | Immersive mode |

---

## Agent 10 — Audio & SFX

Audio agent needs play-testing, spatial audio verification, and volume monitoring.

### Play & Listen
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play (listen to audio in context) |
| `Esc` | Stop |
| `F8` | Eject (move around to test spatial audio) |
| `Pause` | Pause (check audio state) |

### Console (Audio Debug)
| Command | Purpose |
|---------|---------|
| `stat audio` | Audio system stats |
| `stat soundwaves` | Active sound wave info |
| `au.Debug.Sounds 1` | Show active sounds in viewport |
| `au.Debug.SoundAttenuation 1` | Visualize attenuation spheres |
| `slomo 0.5` | Slow-mo (verify footstep timing) |
| `show audioradius` | Display sound attenuation radii |

### Spatial Audio Verification
| Shortcut | Action |
|----------|--------|
| `F8` | Eject player — fly around to check spatialization |
| `Alt + J` | Top-down view (verify audio source placement) |
| `G` | Game view (final audio context) |
| `Alt + 4` | Lit view |

### Asset Browser
| Shortcut | Action |
|----------|--------|
| `Ctrl + P` | Search for sound assets |
| `Ctrl + E` | Edit sound cue/MetaSound |
| `Space` | Preview sound asset in browser |

---

## Agent 11 — UI/UX & Polish

UI/UX agent needs widget editing, play-testing, and resolution/scaling tools.

### Blueprint Editor (Widget Blueprints)
| Shortcut | Action |
|----------|--------|
| `F7` | Compile widget Blueprint |
| `Ctrl + S` | Save |
| `Ctrl + Z / Y` | Undo / Redo |
| `Ctrl + F` | Find in Blueprint |
| `C + Click` | Comment box |
| `B + Click` | Branch |

### Play & Test UI
| Shortcut | Action |
|----------|--------|
| `Alt + P` | Play (see HUD/UI in action) |
| `Esc` | Stop PIE |
| `G` | Toggle Game View (verify UI placement) |
| `F11` | Immersive mode (test fullscreen UI) |
| `Shift + F11` | Fullscreen editor window |

### Console (UI Debug)
| Command | Purpose |
|---------|---------|
| `show hud` | Toggle HUD visibility |
| `stat slate` | Slate/UI performance stats |
| `r.ScreenPercentage 100` | Reset screen percentage |
| `HighResShot 2` | High-res screenshot for UI review |
| `slomo 0.5` | Slow-mo (check UI animations) |

### View Modes
| Shortcut | Action |
|----------|--------|
| `Alt + 4` | Lit (see UI over final scene) |
| `G` | Game view (hide editor overlays) |
| `Ctrl + Shift + H` | FPS counter (UI performance) |

### Content Navigation
| Shortcut | Action |
|----------|--------|
| `Ctrl + P` | Search for widget assets |
| `Ctrl + E` | Edit widget Blueprint |
| `Ctrl + B` | Browse to asset |

---

## Cross-Agent Quick Reference Card

### Universal Shortcuts (All Agents)
| Shortcut | Action |
|----------|--------|
| `Ctrl + Z` | Undo |
| `Ctrl + Y` | Redo |
| `Ctrl + S` | Save |
| `Ctrl + Shift + S` | Save All |
| `Ctrl + P` | Quick Search |
| `Alt + P` | Play In Editor |
| `Esc` | Stop PIE / Deselect |
| `~` | Console |
| `G` | Game View toggle |
| `F2` | Rename selected |
| `Delete` | Delete selected |

### Editor Modes (Shift + #)
| Shortcut | Primary Agent |
|----------|---------------|
| `Shift + 1` | All — Place/Select |
| `Shift + 2` | 07 — Landscape |
| `Shift + 3` | 07 — Foliage |
| `Shift + 4` | 07 — Mesh Paint |
| `Shift + 5` | 07 — Modeling |
| `Shift + 6` | 07 — Fracture |
| `Shift + 7` | 03 — Brush Editing |

### Python Script Execution
All agents can run Python scripts from the editor console:
```
py "D:/documents/Unreal Projects/game_maze/Content/Python/script_name.py"
```
Or via Output Log > Python tab.

---

*Generated from `shortcut_keys_reference.md`. Version-controlled via Git.*
*Last updated: 2026-02-07*
