# UE5 Console Commands Reference — Maze Game

Comprehensive console command reference organized by domain. Access the console
with `~` (tilde) in the editor viewport or during Play In Editor (PIE).

> **Discover more:** Run `dumpconsolecommands` in-console for a full list.
> **Get help:** Run `help <command>` for details on any specific command.

---

## 1. Performance & Profiling

| Command | Description | Agent |
|---------|-------------|-------|
| `stat fps` | Display framerate counter | All |
| `stat unit` | Frame time breakdown (Game, Render, GPU, RHIT) | 00, 01, 06 |
| `stat unitgraph` | Graphical frame time history | 06 |
| `stat memory` | Memory allocation statistics | 06 |
| `stat scenerendering` | Scene rendering cost breakdown | 06, 09 |
| `stat slate` | UI/Slate widget performance | 11 |
| `stat audio` | Audio system statistics | 10 |
| `stat soundwaves` | Active sound wave count and info | 10 |
| `stat navigation` | Navigation/NavMesh statistics | 08 |
| `stat game` | Gameplay thread statistics | 06 |
| `stat engine` | Engine-level statistics | 06 |
| `profilegpu` | Open GPU profiler (frame breakdown) | 06, 09 |
| `obj list` | List all UObjects in memory | 06 |
| `obj gc` | Force garbage collection | 06 |

---

## 2. Navigation & AI

| Command | Description | Agent |
|---------|-------------|-------|
| `RebuildNavigation` | Trigger full NavMesh rebuild | 08 |
| `show navigation` | Toggle NavMesh visualization overlay | 08, 03 |
| `ai.debug` | Toggle AI debugging overlays | 08 |
| `ai.debug.nav` | Navigation-specific AI debug | 08 |

### NavMesh Configuration (DefaultEngine.ini)
```ini
[/Script/NavigationSystem.NavigationSystemV1]
bAutoCreateNavigationData=True

[/Script/NavigationSystem.RecastNavMesh]
AgentRadius=34.0
AgentHeight=144.0
CellSize=10.0
CellHeight=5.0
```

---

## 3. Rendering & Visual

| Command | Description | Agent |
|---------|-------------|-------|
| `r.Lumen.Enabled 1/0` | Toggle Lumen global illumination | 09 |
| `r.VolumetricFog 1/0` | Toggle volumetric fog | 09 |
| `r.ScreenPercentage 100` | Set rendering resolution percentage | 06, 11 |
| `r.SetRes 1920x1080f` | Set resolution (f=fullscreen, w=windowed) | 06 |
| `show fog` | Toggle fog rendering | 09 |
| `show postprocess` | Toggle post-processing effects | 09 |
| `show lighting` | Toggle all lighting | 09 |
| `show directionallight` | Toggle directional lights | 09 |
| `show pointlight` | Toggle point lights | 09 |
| `show spotlight` | Toggle spot lights | 09 |
| `show collision` | Show collision wireframes | 03, 04 |
| `show bounds` | Show actor bounding boxes | 03, 07 |
| `show volumes` | Show trigger/blocking volumes | 03, 05 |
| `show hud` | Toggle HUD rendering | 11 |
| `show audioradius` | Show audio attenuation radii | 10 |
| `HighResShot 2` | Take 2x resolution screenshot | All |
| `HighResShot 4` | Take 4x resolution screenshot | 07, 09 |

---

## 4. Audio

| Command | Description | Agent |
|---------|-------------|-------|
| `au.Debug.Sounds 1` | Show active sounds in viewport | 10 |
| `au.Debug.SoundAttenuation 1` | Visualize attenuation spheres | 10 |
| `au.Debug.SoundCues 1` | Show active sound cue debug info | 10 |
| `stat audio` | Audio system performance stats | 10 |
| `stat soundwaves` | Active sound wave details | 10 |
| `show audioradius` | Display audio attenuation radii in viewport | 10 |
| `AudioThread.EnableAudioThreadWait 0` | Disable audio thread wait (debug) | 10 |

---

## 5. Level & World

| Command | Description | Agent |
|---------|-------------|-------|
| `open <MapName>` | Load a level by name | All |
| `restartlevel` | Restart the current level | 02, 04 |
| `slomo <float>` | Time dilation (0.5 = half speed, 2.0 = double) | 02, 04, 10 |
| `viewmode <mode>` | Set viewport mode (lit, unlit, wireframe) | All |

### Useful Slomo Values
| Value | Purpose |
|-------|---------|
| `slomo 0.1` | Ultra slow-mo for debugging collisions/particles |
| `slomo 0.25` | Detailed observation of animations/footsteps |
| `slomo 0.5` | Half speed for general debugging |
| `slomo 1.0` | Normal speed (reset) |
| `slomo 2.0` | Fast-forward through slow sections |
| `slomo 5.0` | Rapid test iteration |

---

## 6. Cheat Manager (PIE Only)

| Command | Description | Agent |
|---------|-------------|-------|
| `god` | Toggle invincibility | 02 |
| `fly` | Toggle fly mode | 02, 04 |
| `ghost` | No-clip through geometry | 02, 03, 04 |
| `teleport` | Teleport to crosshair location | 02, 03 |
| `walk` | Return to normal walk mode | 02, 04 |

---

## 7. Blueprint & Gameplay Debugging

| Command | Description | Agent |
|---------|-------------|-------|
| `ke * <EventName>` | Fire a Kismet event by name | 05 |
| `ce <EventName>` | Call a custom event on selected actor | 05 |
| `displayall <ClassName> <PropertyName>` | Show property value for all instances | 06 |
| `getall <ClassName> <PropertyName>` | Log property values to output | 06 |
| `showdebug` | Toggle debug overlay for current pawn | 04, 06 |
| `showdebug MOVEMENT` | Character movement debug overlay | 04 |
| `showdebug COLLISION` | Collision debug overlay | 04 |
| `showdebug AI` | AI behavior debug overlay | 08 |

---

## 8. Log & Output

| Command | Description | Agent |
|---------|-------------|-------|
| `log list` | List all log categories | 06 |
| `log <Category> <Level>` | Set log verbosity (e.g., `log LogTemp Verbose`) | 06 |
| `suppress <Category>` | Suppress a log category | 06 |

---

## 9. Maze-Game Specific Commands

Custom console commands implementable via `UFUNCTION(Exec)` or Cheat Manager:

```cpp
// Example: Add to MazeGameMode or a CheatManager subclass
UFUNCTION(Exec)
void RegenerateMaze();

UFUNCTION(Exec)
void SetMazeSize(int32 Rows, int32 Cols);

UFUNCTION(Exec)
void ToggleMinimap();

UFUNCTION(Exec)
void ShowSolution();

UFUNCTION(Exec)
void TeleportToExit();
```

### Planned Custom Commands
| Command | Description | Priority |
|---------|-------------|----------|
| `RegenerateMaze` | Regenerate maze with current config | Must-have |
| `SetMazeSize <R> <C>` | Change maze dimensions on the fly | Nice-to-have |
| `ToggleMinimap` | Show/hide minimap overlay | Nice-to-have |
| `ShowSolution` | Draw path from player to exit | Debug |
| `TeleportToExit` | Instantly move player to exit cell | Debug |
| `ValidateNavMesh` | Run NavMesh solvability check | Debug |

---

## 10. Python Console Integration

Execute Python from the UE console:
```
py "D:/documents/Unreal Projects/game_maze/Content/Python/setup_test_level.py"
```

Or inline:
```
py import unreal; unreal.log("Hello from Python")
```

Python can call console commands:
```python
import unreal
unreal.SystemLibrary.execute_console_command(
    unreal.EditorLevelLibrary.get_editor_world(),
    "RebuildNavigation"
)
```

---

*Derived from `shortcut_keys_reference.md` and UE5 documentation.*
*Last updated: 2026-02-07*
