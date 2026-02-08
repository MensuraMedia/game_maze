# AI Editor Operations Guide

Practical instructions for AI-administered Unreal Editor control. Covers configuration,
log monitoring, script production, and the user-assisted command execution workflow.

> **Project:** `game_maze` (UE5.7)
> **Editor Path:** `D:\documents\Unreal Projects\game_maze`
> **Planning Repo:** `D:\projects\maze`

---

## 1. Unreal Editor Configuration for AI Administration

### 1.1 Required Plugins

Enable these before any AI interaction (one-time setup):

| Plugin | Path | Purpose |
|--------|------|---------|
| Python Editor Script | Edit > Plugins > Scripting | Execute Python via `py` console command |
| Editor Scripting Utilities | Edit > Plugins > Scripting | Extended editor Python APIs |
| Remote Control API | Edit > Plugins > Virtual Production | HTTP/WebSocket control (optional) |

After enabling, restart the editor.

### 1.2 Console Key Verification

The `~` (tilde) key must open the console. If it doesn't (non-US keyboard):

**Fix via config file** (`D:\documents\Unreal Projects\game_maze\Config\DefaultEngine.ini`):
```ini
[/Script/Engine.InputSettings]
ConsoleKey=Tilde
```

**Verify:** Press `~` in the viewport — a text input bar should appear at the bottom.

### 1.3 Recommended Custom Keybindings

Set these once via **Edit > Editor Preferences > Keyboard Shortcuts**:

| Action | Suggested Binding | Purpose |
|--------|-------------------|---------|
| Output Log | `Alt + O` | Quick access to execution results |
| Content Browser | `Ctrl + Space` | Fast asset search |

### 1.4 Python Script Directory

All AI-generated Python scripts are placed in:
```
D:\documents\Unreal Projects\game_maze\Content\Python\
```

UE auto-discovers scripts in `Content/Python/`. They can be executed via:
```
py "D:/documents/Unreal Projects/game_maze/Content/Python/script_name.py"
```

### 1.5 Project GameMode Configuration

The level's World Settings must have `Default GameMode` set to `MazeGameMode`.
AI sets this via Python:
```python
import unreal
world = unreal.get_editor_subsystem(unreal.UnrealEditorSubsystem).get_editor_world()
gm_class = unreal.load_class(None, '/Script/game_maze.MazeGameMode')
world.get_world_settings().set_editor_property('default_game_mode', gm_class)
```

**Note:** The property name is `default_game_mode` (not `game_mode_override`).
C++ classes use `unreal.load_class(None, '/Script/module.ClassName')`, not
`load_blueprint_class` which only works for Blueprint assets.

---

## 2. Reading and Monitoring Log Files

### 2.1 Log File Location

The UE output log is written to disk in real-time:
```
D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log
```

### 2.2 AI Reading the Log

The AI reads the log directly from the filesystem using PowerShell:

**Read last N lines:**
```powershell
powershell -Command "Get-Content 'D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log' -Tail 30"
```

**Filter for game-relevant messages (exclude Epic telemetry noise):**
```powershell
powershell -Command "Get-Content 'D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log' | Select-String -Pattern 'LogTemp|LogPython|PIE:|Maze|Error|Warning.*maze' | Select-Object -Last 30"
```

**Filter for errors only:**
```powershell
powershell -Command "Get-Content 'D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log' | Select-String -Pattern 'Error' | Select-String -NotMatch 'LogHttp' | Select-Object -Last 20"
```

**Watch log in real-time (tail -f equivalent):**
```powershell
powershell -Command "Get-Content 'D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log' -Wait -Tail 10"
```

### 2.3 Log Message Categories

| Log Prefix | Source | Relevance |
|------------|--------|-----------|
| `LogTemp` | Our C++ code (`UE_LOG(LogTemp, ...)`) | High — game system output |
| `LogPython` | Python script execution | High — script results/errors |
| `LogMazeMinimap` | Custom log category | High — minimap system |
| `PIE:` | Play In Editor session | High — play session events |
| `LogLoad` | Asset and class loading | Medium — verify class usage |
| `LogEditor` | Editor actions (spawn, delete) | Medium — actor operations |
| `LogPlayLevel` | PIE initialization | Medium — session setup |
| `LogHttp` | Epic telemetry | Ignore — network noise |
| `LogSavePackage` | Autosave activity | Ignore — file operations |
| `LogDerivedDataCache` | DDC maintenance | Ignore — cache cleanup |

### 2.4 Verification Patterns

After executing a command, the AI checks the log for expected output:

```powershell
# Verify a specific action succeeded
powershell -Command "Get-Content '...game_maze.log' | Select-String 'MazeGenerator: Generated' | Select-Object -Last 1"

# Check for Python errors
powershell -Command "Get-Content '...game_maze.log' | Select-String 'LogPython: Error' | Select-Object -Last 5"

# Verify PIE session started
powershell -Command "Get-Content '...game_maze.log' | Select-String 'Game class is' | Select-Object -Last 1"
```

---

## 3. Producing and Managing Scripts

### 3.1 Starting Point: End-to-End Flow

This is the complete sequence from the AI's perspective. Every editor interaction
follows this pattern.

**Step A — AI creates the script file on disk:**

The AI uses its file-writing tools (Write tool, or PowerShell) to create a `.py`
script in the UE project's Python directory:

```powershell
# PowerShell: Write a Python script to disk
powershell -Command "Set-Content -Path 'D:\documents\Unreal Projects\game_maze\Content\Python\my_action.py' -Value @'
import unreal

# Your automation logic here
sub = unreal.get_editor_subsystem(unreal.EditorActorSubsystem)
actors = sub.get_all_level_actors()
unreal.log(f'Found {len(actors)} actors in level')
'@"
```

Or for quick one-liners, skip the file and go straight to Step B.

**Step B — AI copies the execution command to the system clipboard:**

```powershell
# PowerShell: Copy the py execution command to clipboard
powershell -Command "Set-Clipboard -Value 'py \"D:/documents/Unreal Projects/game_maze/Content/Python/my_action.py\"'"
```

For one-liners (no script file needed):
```powershell
# PowerShell: Copy an inline Python command to clipboard
powershell -Command "Set-Clipboard -Value 'py import unreal; unreal.log(\"hello from AI\")'"
```

For console commands (no Python):
```powershell
# PowerShell: Copy a console command to clipboard
powershell -Command "Set-Clipboard -Value 'stat fps'"
```

**Step C — User executes in the UE Editor:**

```
Alt + Tab      → Switch to UE Editor
~              → Open console
Ctrl + V       → Paste command
Enter          → Execute
~              → Close console
```

**Step D — AI verifies the result by reading the log file:**

```powershell
# PowerShell: Read the last 20 game-relevant log lines
powershell -Command "Get-Content 'D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log' | Select-String -Pattern 'LogTemp|LogPython|PIE:|Maze' | Select-Object -Last 20"
```

**Complete example — spawn an actor and verify:**

```powershell
# Step A: Write the script
powershell -Command "Set-Content -Path 'D:\documents\Unreal Projects\game_maze\Content\Python\spawn_light.py' -Value @'
import unreal
sub = unreal.get_editor_subsystem(unreal.EditorActorSubsystem)
light = sub.spawn_actor_from_class(unreal.PointLight, unreal.Vector(150, 150, 250), unreal.Rotator(0, 0, 0))
if light:
    light.point_light_component.set_intensity(8000.0)
    unreal.log(f'Spawned point light: {light.get_name()}')
'@"

# Step B: Copy execution command to clipboard
powershell -Command "Set-Clipboard -Value 'py \"D:/documents/Unreal Projects/game_maze/Content/Python/spawn_light.py\"'"

# Step C: User does  Alt+Tab → ~ → Ctrl+V → Enter → ~

# Step D: AI verifies
powershell -Command "Get-Content 'D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log' | Select-String 'Spawned point light' | Select-Object -Last 1"
```

### 3.2 Workflow Diagram

```
┌──────────────────┐    ┌────────────────────┐    ┌─────────────────┐
│ A. AI writes .py │───→│ B. AI copies exec  │───→│ C. User does    │
│ to Content/Python│    │ cmd to clipboard   │    │ Alt+Tab ~ Ctrl+V│
│ via PowerShell   │    │ via Set-Clipboard  │    │ Enter ~         │
└──────────────────┘    └────────────────────┘    └────────┬────────┘
                                                           │
                                                  ┌────────┴────────┐
                                                  │ D. AI reads log │
                                                  │ via Get-Content │
                                                  │ to verify       │
                                                  └─────────────────┘
```

### 3.3 Script Types

**Setup Scripts** — Run once to configure the level:
```
Content/Python/setup_test_level.py    — Spawn maze actors, set GameMode
Content/Python/cleanup_template.py    — Remove default template content
```

**Diagnostic Scripts** — Run to inspect state:
```
Content/Python/verify_setup.py        — Check all actors present
Content/Python/ue_bridge.py           — Reusable function library
```

**One-liner Commands** — Quick actions via console paste:
```
py import unreal; unreal.log(f'Actors: {len(unreal.get_editor_subsystem(unreal.EditorActorSubsystem).get_all_level_actors())}')
```

### 3.4 Script Writing Rules

1. **Use `unreal.get_editor_subsystem()`** — not deprecated `EditorLevelLibrary`
2. **Use `unreal.load_class(None, '/Script/module.Class')`** — for C++ classes
3. **Use `set_editor_property('snake_case_name', value)`** — property names are snake_case
4. **Always `unreal.log()` results** — so the AI can verify via the log file
5. **Handle missing actors gracefully** — check `if actor:` before using

### 3.5 Clipboard Command Preparation

The AI copies commands to the system clipboard using PowerShell:

**Single command:**
```powershell
powershell -Command "Set-Clipboard -Value 'py \"D:/documents/Unreal Projects/game_maze/Content/Python/script.py\"'"
```

**Multi-line command block (use backtick-n for newlines):**
```powershell
powershell -Command "Set-Clipboard -Value 'stat fps`nstat unit`nshow navigation'"
```

**Command with quotes (escape inner quotes):**
```powershell
powershell -Command "Set-Clipboard -Value 'py import unreal; unreal.log(\"hello\")'"
```

---

## 4. User Command Execution Instructions

### 4.1 The Standard Sequence

When the AI says "command is on your clipboard", follow these steps:

```
Step 1:  Alt + Tab         → Switch to the Unreal Editor window
Step 2:  ~  (tilde key)    → Open the console bar (bottom of viewport)
Step 3:  Ctrl + V          → Paste the command from clipboard
Step 4:  Enter             → Execute the command
Step 5:  ~  (tilde key)    → Close the console bar
```

**Timing:** Each step is instantaneous. No waiting needed between steps.

### 4.2 Visual Confirmation

After executing, you should see:

| What | Where | Meaning |
|------|-------|---------|
| Text in console bar | Bottom of viewport | Command was pasted |
| Console bar disappears | Bottom of viewport | Console closed (after `~`) |
| Log messages | Output Log window | Command executed |
| Viewport changes | Main viewport | Visual commands took effect |

### 4.3 Common Sequences

**Execute a Python script:**
```
Alt+Tab → ~ → Ctrl+V → Enter → ~
```

**Play the game:**
```
Alt+P
```

**Stop playing:**
```
Esc
```

**Play with debug overlays:**
```
Alt+Tab → ~ → Ctrl+V → Enter → ~     (paste debug commands)
Alt+P                                   (play)
```

**Take a screenshot during play:**
```
F9
```

**Eject from player to fly around:**
```
F8
```

**Top-down view of the maze:**
```
Alt+J
```

**Return to perspective view:**
```
Alt+G
```

### 4.4 Troubleshooting

| Problem | Solution |
|---------|----------|
| `~` doesn't open console | Try the key left of `1` on your keyboard. If still nothing, go to Edit > Editor Preferences > Keyboard Shortcuts > search "Console" and rebind. |
| `py` says "unrecognized" | Python plugin not enabled. Go to Edit > Plugins > search "Python" > enable "Python Editor Script Plugin" > restart editor. |
| Paste shows wrong text | Something else copied over the clipboard. Return to the AI terminal and let it re-copy the command. |
| Command executes but nothing happens | Check the Output Log (Window > Developer Tools > Output Log) for error messages. Report them to the AI. |
| Black screen during play | The player spawned in the wrong location. Press `Esc` to stop, then run the cleanup script to remove template content. |
| Editor froze | Wait 10-15 seconds — large operations (NavMesh rebuild, maze generation) can temporarily block. If truly frozen, use Task Manager to end `UnrealEditor.exe`. |

### 4.5 Reporting Results to the AI

After executing a command, tell the AI:
- **"Done"** — if no visible errors occurred
- **Paste the error text** — if the Output Log shows errors
- **Describe what you see** — if the viewport changed unexpectedly

The AI can also read results directly from the log file without user input:
```
D:\documents\Unreal Projects\game_maze\Saved\Logs\game_maze.log
```

---

## 5. AI Administration Checklist

### First-Time Setup
- [ ] UE Editor open with `game_maze` project
- [ ] Python Editor Script Plugin enabled
- [ ] Console opens with `~` key
- [ ] GameMode set to `MazeGameMode` (via Python command)
- [ ] Template content cleaned up (via cleanup script)
- [ ] MazeGenerator, MazeAtmosphere, MazeAmbientAudio, MazeVisualEffects spawned

### Before Each Play Test
- [ ] Check log for errors: `Select-String 'Error' | Select-String -NotMatch 'LogHttp'`
- [ ] Verify actors present: run `verify_setup.py` or `ue_bridge.diagnostic()`
- [ ] Save level: `Ctrl+S` in editor or `ue_bridge.save_level()` via Python

### After Each Play Test
- [ ] Read log for gameplay output: filter for `LogTemp|LogPython|PIE:`
- [ ] Check for crashes or assertion failures
- [ ] Note any visual issues reported by user

---

## 6. File Reference

| File | Location | Purpose |
|------|----------|---------|
| `game_maze.log` | `Saved/Logs/` | Live output log (AI reads this) |
| `setup_test_level.py` | `Content/Python/` | Spawn maze actors + set GameMode |
| `cleanup_template.py` | `Content/Python/` | Remove default template content |
| `verify_setup.py` | `Content/Python/` | Diagnostic: check all actors present |
| `ue_bridge.py` | `Content/Python/` | Reusable function library |
| `diagnostic_play.py` | `Content/Python/` | Enable debug overlays before play |
| `DefaultEngine.ini` | `Config/` | Engine settings (console key, NavMesh) |
| `DefaultGame.ini` | `Config/` | Project name, packaging settings |

---

*Part of the maze game AI administration toolkit.*
*See also: `clipboard_console_automation.md`, `agent_shortcut_reference.md`, `console_commands_reference.md`*
*Last updated: 2026-02-08*
