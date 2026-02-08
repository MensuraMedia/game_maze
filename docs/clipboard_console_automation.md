# Clipboard-Console-Shortcut Automation Reference

Advanced reference for AI-agent-to-UE-Editor direct control using the clipboard pipeline.
This document defines the core technique, concrete scenarios, and per-agent workflows
for controlling the Unreal Editor in real-time through generated commands, scripts, and
shortcut key sequences.

> **Source:** `shortcut_keys_with_code_collaboration.md` (production-tested methodology)
> **Prerequisite:** UE Editor open with `game_maze` project loaded
> **Python Plugin:** Edit > Plugins > Scripting > "Python Editor Script Plugin" (must be enabled)

---

## 1. Core Technique: The Clipboard Pipeline

The fundamental automation flow for any agent:

```
┌─────────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│  Agent Generates │───→│  Copy to     │───→│  Focus UE    │───→│  Paste &     │
│  Command/Script  │    │  Clipboard   │    │  Editor      │    │  Execute     │
└─────────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                                                                       │
                                                               ┌───────┴───────┐
                                                               │  Verify via   │
                                                               │  Output Log   │
                                                               └───────────────┘
```

### Execution Sequence (Step-by-Step)

| Step | Action | Key/Method | Delay |
|------|--------|------------|-------|
| 1 | Agent generates command block | Code generation | — |
| 2 | Copy to system clipboard | `pyperclip.copy()` or OS clipboard API | — |
| 3 | Focus UE Editor window | Window title match: `"Unreal Editor"` | 200ms |
| 4 | Open console | `~` (tilde) | 150ms |
| 5 | Paste command(s) | `Ctrl + V` | 100ms |
| 6 | Execute | `Enter` | 300ms |
| 7 | Close console | `~` | 100ms |
| 8 | Verify (optional) | Open Output Log, read results | 500ms |

### Multi-Command Pasting

UE console supports multi-line paste. Agents can chain 15-30 commands in a single
clipboard payload — each line executes sequentially:

```
r.Lumen.Enabled 1
r.VolumetricFog 1
stat fps
stat unit
```

All four commands execute in order from a single paste + Enter.

---

## 2. Command Categories Available via Console

| Category | Prefix/Pattern | Example |
|----------|---------------|---------|
| Rendering settings | `r.*` | `r.Lumen.Enabled 1` |
| Show flags | `show *` | `show navigation` |
| Scalability | `sg.*` | `sg.ShadowQuality 4` |
| Statistics | `stat *` | `stat fps` |
| Audio debug | `au.*` | `au.Debug.Sounds 1` |
| Navigation | `Rebuild*` | `RebuildNavigation` |
| Python execution | `py *` | `py setup_test_level.py` |
| Cheat commands | Direct | `ghost`, `god`, `fly` |
| Time control | `slomo` | `slomo 0.5` |
| Screenshot | `HighResShot` | `HighResShot 4` |

---

## 3. Scenario Catalog

### Scenario 1: Post-Build Smoke Test
**Agent:** 00 Conductor / 01 Team Lead
**Trigger:** After C++ hot-reload or fresh editor launch
**Goal:** Verify the game is functional after a code change

**Command Block:**
```
py import unreal; unreal.log('=== SMOKE TEST START ===')
stat fps
stat unit
stat memory
show navigation
show collision
```

**Shortcut Sequence After Paste:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste → `Enter` → `~` | Execute diagnostics |
| 2 | `Alt + P` | Start PIE session |
| 3 | *(wait 3s for level load)* | |
| 4 | `F8` | Eject to inspect from above |
| 5 | `Alt + J` | Top-down view |
| 6 | `F9` | Screenshot for verification |
| 7 | `Esc` | Stop PIE |

**Verification:** Check Output Log for error count, confirm FPS > 30, confirm NavMesh visible.

---

### Scenario 2: Maze Regeneration & Validation Cycle
**Agent:** 03 Level Designer / 08 AI Navigation
**Trigger:** Testing different maze configurations
**Goal:** Regenerate maze, rebuild NavMesh, validate solvability — all without touching the mouse

**Command Block (Phase 1 — Generate):**
```
py import unreal; gen = unreal.GameplayStatics.get_actor_of_class(unreal.EditorLevelLibrary.get_editor_world(), unreal.MazeGenerator); gen.clear_maze() if gen else unreal.log_warning('No MazeGenerator found')
py import unreal; gen = unreal.GameplayStatics.get_actor_of_class(unreal.EditorLevelLibrary.get_editor_world(), unreal.MazeGenerator); gen.generate_maze() if gen else None
```

**Command Block (Phase 2 — Validate):**
```
RebuildNavigation
show navigation
py import unreal; gen = unreal.GameplayStatics.get_actor_of_class(unreal.EditorLevelLibrary.get_editor_world(), unreal.MazeGenerator); unreal.log(f'Solvable: {gen.validate_solvability()}') if gen else None
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste Phase 1 → `Enter` → `~` | Regenerate maze |
| 2 | *(wait 1s)* | Allow geometry to spawn |
| 3 | `~` → paste Phase 2 → `Enter` → `~` | Rebuild nav + validate |
| 4 | `Alt + J` | Top-down view to inspect layout |
| 5 | `Alt + 2` | Wireframe to see NavMesh through walls |
| 6 | `Home` | Zoom to fit entire maze |
| 7 | `F9` | Screenshot for records |

---

### Scenario 3: Lighting Quality Review
**Agent:** 09 Lighting & Atmosphere
**Trigger:** After adjusting MazeAtmosphere or MazeTorchLight settings
**Goal:** Cycle through all relevant view modes and capture screenshots

**Command Block (Cinematic Preset):**
```
sg.AntiAliasingQuality 4
sg.GlobalIlluminationQuality 4
sg.ReflectionQuality 4
sg.ShadowQuality 4
sg.TextureQuality 4
r.Lumen.Enabled 1
r.VolumetricFog 1
r.ScreenPercentage 150
r.TemporalAA.Upsampling 1
```

**Shortcut Sequence (View Mode Sweep):**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste preset → `Enter` → `~` | Apply cinematic quality |
| 2 | `Alt + 4` | Lit view — final look |
| 3 | `F9` | Screenshot: Lit |
| 4 | `Alt + 5` | Detail Lighting |
| 5 | `F9` | Screenshot: Detail Lighting |
| 6 | `Alt + 6` | Lighting Only |
| 7 | `F9` | Screenshot: Lighting Only |
| 8 | `Alt + 7` | Light Complexity |
| 9 | `F9` | Screenshot: Light Complexity |
| 10 | `Alt + 4` | Return to Lit |
| 11 | `G` | Toggle Game View (final player perspective) |
| 12 | `F9` | Screenshot: Game View |

**Verification:** Compare screenshots. Light Complexity view should show no red (overdrawn) areas.

---

### Scenario 4: Audio Spatialization Debug
**Agent:** 10 Audio & SFX
**Trigger:** After placing MazeAmbientAudio or adjusting attenuation
**Goal:** Visualize audio sources and verify spatial falloff

**Command Block:**
```
au.Debug.Sounds 1
au.Debug.SoundAttenuation 1
show audioradius
stat audio
stat soundwaves
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste → `Enter` → `~` | Enable audio debug overlays |
| 2 | `Alt + P` | Play (audio only active during PIE) |
| 3 | `F8` | Eject to fly around |
| 4 | Move through maze with `RMB + WASD` | Listen to spatial transitions |
| 5 | `Alt + J` | Top-down — see all attenuation radii |
| 6 | `F9` | Screenshot attenuation layout |
| 7 | `Esc` | Stop PIE |

**Verification:** Attenuation spheres should cover corridors without excessive overlap. `stat audio`
shows active voice count stays under budget.

---

### Scenario 5: Character Movement Stress Test
**Agent:** 04 Character Controller
**Trigger:** After modifying MazeCharacter walk/sprint speeds or collision
**Goal:** Verify no clipping, smooth framerate, correct footstep surfaces

**Command Block:**
```
show collision
showdebug MOVEMENT
stat fps
stat unit
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste → `Enter` → `~` | Enable movement debug |
| 2 | `Alt + P` | Play |
| 3 | WASD movement through corridors | Test collision response |
| 4 | Sprint (Shift) along walls | Test high-speed collision |
| 5 | Run into corners deliberately | Test stuck-prevention |
| 6 | `Pause` | Freeze to inspect collision state |
| 7 | `F9` | Screenshot debug overlay |
| 8 | `Esc` | Stop |

**Console follow-up (if clipping detected):**
```
show bounds
slomo 0.25
```
Then replay at quarter-speed to isolate the exact collision frame.

---

### Scenario 6: HUD & Minimap Visual Test
**Agent:** 11 UI/UX & Polish
**Trigger:** After modifying MazeHUD, MazeHUDWidget, or MazeMinimap
**Goal:** Verify HUD renders correctly at different resolutions

**Command Block (1080p test):**
```
r.SetRes 1920x1080w
show hud
stat slate
```

**Command Block (4K test):**
```
r.SetRes 3840x2160w
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste 1080p block → `Enter` → `~` | Set resolution |
| 2 | `Alt + P` | Play |
| 3 | `G` | Game View (verify HUD placement) |
| 4 | `F9` | Screenshot: 1080p HUD |
| 5 | `Esc` | Stop |
| 6 | `~` → paste 4K block → `Enter` → `~` | Switch to 4K |
| 7 | `Alt + P` | Play |
| 8 | `F9` | Screenshot: 4K HUD |
| 9 | `Esc` | Stop |

**Verification:** Minimap stays in top-right corner. Timer/score text remains readable. No
overlapping elements. `stat slate` shows widget render time < 1ms.

---

### Scenario 7: Full Python Script Execution
**Agent:** Any (via Python automation pipeline)
**Trigger:** Complex multi-step operations that exceed single-line console commands
**Goal:** Execute a full Python script from disk via clipboard pipeline

**Command (single line):**
```
py "D:/documents/Unreal Projects/game_maze/Content/Python/setup_test_level.py"
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste → `Enter` | Execute script |
| 2 | *(wait 2s)* | Script spawns actors |
| 3 | `~` (close console) | |
| 4 | `Home` | Zoom to fit all new actors |
| 5 | `Ctrl + S` | Save level with new actors |

**Advanced — Inline Python via Console:**
For quick operations without a script file, agents can paste Python directly:
```
py import unreal; [unreal.log(f'{a.get_name()}: {a.get_class().get_name()}') for a in unreal.EditorLevelLibrary.get_all_level_actors()]
```

---

### Scenario 8: Blueprint Compile & Test Cycle
**Agent:** 05 Blueprint Agent
**Trigger:** After modifying Blueprint event graphs
**Goal:** Compile all Blueprints and verify no errors

**Shortcut Sequence (No Console Needed):**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `Ctrl + P` | Open quick search |
| 2 | Type Blueprint name → `Enter` | Open specific Blueprint |
| 3 | `F7` | Compile |
| 4 | *(check for errors in log)* | |
| 5 | `Ctrl + S` | Save Blueprint |
| 6 | `Ctrl + Shift + F` | Search all Blueprints for broken references |
| 7 | `Alt + P` | Play to test |
| 8 | `Esc` | Stop |

**Console verification after compile:**
```
py import unreal; assets = unreal.EditorAssetLibrary.list_assets('/Game/', recursive=True); bps = [a for a in assets if 'Blueprint' in str(unreal.EditorAssetLibrary.find_asset_data(a).asset_class)]; unreal.log(f'Total Blueprints: {len(bps)}')
```

---

### Scenario 9: NavMesh Coverage Inspection
**Agent:** 08 AI & Navigation
**Trigger:** After maze generation or geometry changes
**Goal:** Verify NavMesh fully covers all corridors

**Command Block:**
```
RebuildNavigation
show navigation
stat navigation
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste → `Enter` → `~` | Rebuild and show NavMesh |
| 2 | `Alt + J` | Top-down view |
| 3 | `Alt + 2` | Wireframe (see NavMesh through walls) |
| 4 | `Home` | Zoom to fit entire maze |
| 5 | `F9` | Screenshot: NavMesh coverage |
| 6 | `Alt + 4` | Return to Lit view |
| 7 | `Alt + G` | Perspective view |
| 8 | `Ctrl + 1` | Bookmark this camera angle |

**Gap Detection (Python):**
```
py import unreal; nav = unreal.NavigationSystemV1.get_navigation_system(unreal.EditorLevelLibrary.get_editor_world()); unreal.log(f'NavMesh valid: {nav is not None}')
```

---

### Scenario 10: Performance Profiling Session
**Agent:** 06 C++ Programmer / 00 Conductor
**Trigger:** Before milestone builds or when framerate drops below target
**Goal:** Capture comprehensive performance profile

**Command Block (Enable All Stats):**
```
stat fps
stat unit
stat unitgraph
stat scenerendering
stat memory
stat game
profilegpu
```

**Command Block (Cleanup After):**
```
stat fps
stat unit off
stat unitgraph off
stat scenerendering off
stat memory off
stat game off
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste enable block → `Enter` → `~` | Turn on all profiling |
| 2 | `Alt + P` | Play |
| 3 | Walk through maze for 30s | Gather representative data |
| 4 | `Ctrl + Shift + ,` | Open GPU Profiler |
| 5 | `F9` | Screenshot: full profile overlay |
| 6 | `Esc` | Stop PIE |
| 7 | `~` → paste cleanup block → `Enter` → `~` | Reset stat display |

---

### Scenario 11: Hot Reload + Immediate Validation
**Agent:** 06 C++ Programmer / 01 Team Lead
**Trigger:** After editing C++ source files externally
**Goal:** Compile, reload, and verify without restarting the editor

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `Ctrl + Alt + F11` | Trigger Live Coding hot-reload |
| 2 | *(wait for "Live Coding Succeeded")* | |
| 3 | `Ctrl + S` | Save level |

**If Live Coding fails (header changes):**
| Step | Action | Method |
|------|--------|--------|
| 1 | Close editor | User action |
| 2 | Build externally | `dotnet UnrealBuildTool.dll` via CLI |
| 3 | Relaunch editor | `UnrealEditor.exe game_maze.uproject` |
| 4 | Execute smoke test | Scenario 1 |

**Post-reload validation command:**
```
py import unreal; unreal.log(f'Module loaded, actor count: {len(unreal.EditorLevelLibrary.get_all_level_actors())}')
```

---

### Scenario 12: Collectible Placement Audit
**Agent:** 05 Blueprint Agent / 03 Level Designer
**Trigger:** After maze generation to verify collectible spawn positions
**Goal:** Confirm all collectibles are reachable and properly placed

**Command Block:**
```
py import unreal; actors = unreal.EditorLevelLibrary.get_all_level_actors(); collectibles = [a for a in actors if 'Collectible' in a.get_class().get_name()]; unreal.log(f'Collectibles found: {len(collectibles)}'); [unreal.log(f'  {c.get_name()} at {c.get_actor_location()}') for c in collectibles]
show collision
show bounds
```

**Shortcut Sequence:**
| Step | Key | Purpose |
|------|-----|---------|
| 1 | `~` → paste → `Enter` → `~` | List all collectibles |
| 2 | `Alt + J` | Top-down to see spatial distribution |
| 3 | `Home` | Zoom to fit |
| 4 | `F9` | Screenshot: collectible layout |
| 5 | `Alt + P` | Play |
| 6 | `ghost` (in console) | No-clip to reach each collectible |
| 7 | `Esc` | Stop |

---

## 4. Compound Workflow Patterns

### Pattern A: Generate → Build → Test → Verify (Full Cycle)

Used by the Conductor for end-to-end integration:

```
Step 1: Agent writes C++ code to disk
Step 2: Build externally (dotnet UBT) or hot-reload (Ctrl+Alt+F11)
Step 3: Console paste: "py setup_test_level.py" → Enter
Step 4: Alt+P (Play)
Step 5: F8 (Eject) → Alt+J (Top-down) → F9 (Screenshot)
Step 6: Esc (Stop)
Step 7: Console paste: "stat fps\nstat unit" → check performance
Step 8: Console paste: "show navigation" → verify NavMesh
```

### Pattern B: Iterative Design Loop (Rapid Cycle)

Used by Game Director and Level Designer for fast design iteration:

```
Loop:
  1. Modify config value (Python one-liner via console)
  2. Regenerate maze (py command)
  3. Alt+P → play 15 seconds → Esc
  4. Adjust → repeat
```

**Example config change via console:**
```
py import unreal; gen = unreal.GameplayStatics.get_actor_of_class(unreal.EditorLevelLibrary.get_editor_world(), unreal.MazeGenerator); gen.set_editor_property('config', gen.get_editor_property('config')); unreal.log('Config updated')
```

### Pattern C: Multi-Agent Handoff

When one agent's output feeds another:

```
Agent 03 (Level Designer):
  1. Generates maze geometry
  2. Pastes: "RebuildNavigation"

Agent 08 (AI/Nav) takes over:
  3. Pastes: "show navigation\nstat navigation"
  4. Verifies coverage

Agent 09 (Lighting) takes over:
  5. Pastes cinematic quality preset
  6. Cycles view modes (Alt+4 through Alt+8)
  7. Places lights via Python script

Agent 10 (Audio) takes over:
  8. Pastes: "au.Debug.Sounds 1\nshow audioradius"
  9. Verifies attenuation coverage
```

---

## 5. Error Handling & Recovery

### Console Command Failures

| Symptom | Recovery |
|---------|----------|
| `Command not recognized` | Verify command spelling; run `dumpconsolecommands` |
| Console doesn't open with `~` | Check keyboard layout; rebind in DefaultEngine.ini |
| Python command fails | Check Python plugin is enabled; verify `py` prefix |
| Multi-line paste truncated | Split into smaller batches (max ~15 commands) |
| `Access denied` on property | Property may be C++ private; use Python `set_editor_property` |

### Output Log Verification

After any console sequence, verify results:

| Step | Key | Purpose |
|------|-----|---------|
| 1 | Open Output Log (bind to `Alt + O`) | View execution results |
| 2 | `Ctrl + End` | Scroll to latest output |
| 3 | `Ctrl + F` | Search for "Error", "Warning", or expected keywords |

### Recovery Commands

```
# Reset rendering to defaults
sg.AntiAliasingQuality 3
sg.GlobalIlluminationQuality 3
r.ScreenPercentage 100

# Reset time
slomo 1.0

# Clear debug overlays
show collision
show navigation
show bounds
show audioradius

# Force cleanup
obj gc
FlushRenderingCommands
```

---

## 6. Clipboard Pipeline Implementation

### For Claude Code (This Agent System)

Claude Code agents generate commands as strings, then instruct the user to paste them
into the UE console. The user's workflow:

1. Agent outputs a command block (fenced code block)
2. User copies the block (`Ctrl + C` in terminal)
3. User switches to UE Editor (`Alt + Tab`)
4. User presses `~` → `Ctrl + V` → `Enter` → `~`

### For Automated Agents (pyautogui / Computer Use)

```python
import pyperclip
import pyautogui
import time

def execute_ue_command(command_block, window_title="Unreal Editor"):
    """Copy command to clipboard and execute in UE console."""
    # Step 1: Copy to clipboard
    pyperclip.copy(command_block)

    # Step 2: Focus UE Editor
    windows = pyautogui.getWindowsWithTitle(window_title)
    if windows:
        windows[0].activate()
        time.sleep(0.2)

    # Step 3: Open console, paste, execute
    pyautogui.press('`')       # Open console (~)
    time.sleep(0.15)
    pyautogui.hotkey('ctrl', 'v')  # Paste
    time.sleep(0.1)
    pyautogui.press('enter')   # Execute
    time.sleep(0.3)
    pyautogui.press('`')       # Close console

# Usage
execute_ue_command("stat fps\nstat unit\nshow navigation")
```

### For Python Inside UE (No Clipboard Needed)

When running Python directly inside the editor, use `unreal.SystemLibrary`:

```python
import unreal

def run_console_command(command):
    """Execute a console command from within UE Python."""
    world = unreal.EditorLevelLibrary.get_editor_world()
    unreal.SystemLibrary.execute_console_command(world, command)

# Usage
run_console_command("RebuildNavigation")
run_console_command("show navigation")
run_console_command("stat fps")
```

---

## 7. Quick Reference Card

### Golden Keys
| Key | Purpose |
|-----|---------|
| `~` | Open/close console (most important key) |
| `Ctrl + V` | Paste command(s) |
| `Enter` | Execute |
| `Alt + P` | Play In Editor |
| `Esc` | Stop PIE |
| `F8` | Eject/Possess |
| `F9` | Screenshot |
| `G` | Game View |

### Essential Console Prefixes
| Prefix | Domain |
|--------|--------|
| `py ` | Execute Python |
| `r.` | Rendering settings |
| `sg.` | Scalability group |
| `stat ` | Statistics overlay |
| `show ` | Toggle visibility flag |
| `au.` | Audio debug |

### Timing Guidelines
| Action | Recommended Delay |
|--------|-------------------|
| After focusing window | 200ms |
| After opening console (`~`) | 150ms |
| After paste (`Ctrl+V`) | 100ms |
| After execute (`Enter`) | 300ms |
| After closing console (`~`) | 100ms |
| After Play (`Alt+P`) | 2000-3000ms (level load) |
| After NavMesh rebuild | 1000-5000ms (depends on size) |

---

*Derived from `shortcut_keys_with_code_collaboration.md` and production AI-UE workflows.*
*Supplements: `agent_shortcut_reference.md`, `console_commands_reference.md`*
*Last updated: 2026-02-07*
