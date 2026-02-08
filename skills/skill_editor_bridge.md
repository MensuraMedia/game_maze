# Skill: Editor Bridge — Direct AI-to-UE Interaction

## ID
`editor-bridge`

## Description
Execute console commands, Python scripts, and shortcut key sequences in the live
UE Editor from the AI agent pipeline. Uses the clipboard-console automation pattern
for reliable, repeatable editor control.

## Agents
Primary: Agent 00 (Conductor), Agent 01 (Team Lead) | Available to: All agents

## Architecture

```
┌──────────────┐    ┌─────────────┐    ┌───────────────┐    ┌──────────────┐
│ Claude Code   │───→│ PowerShell  │───→│ UE Editor     │───→│ Output Log   │
│ (generates)   │    │ (clipboard  │    │ (~ console    │    │ (verify)     │
│               │    │  + SendKeys)│    │  or Python)   │    │              │
└──────────────┘    └─────────────┘    └───────────────┘    └──────────────┘
```

## Prerequisites
- UE Editor open with `game_maze` project loaded
- Python Editor Script Plugin enabled (Edit > Plugins > Scripting)
- `ue_bridge.py` available at `Content/Python/ue_bridge.py`

## Interaction Methods (Priority Order)

### Method 1: PowerShell Clipboard + User Shortcut (Most Reliable)
Agent copies command to clipboard via PowerShell, user executes `~ → Ctrl+V → Enter → ~`.

```powershell
# Copy single command
powershell -Command "Set-Clipboard -Value 'py ue_bridge; ue_bridge.diagnostic()'"

# Copy multi-line block
powershell -Command "Set-Clipboard -Value 'stat fps`nstat unit`nshow navigation'"
```

User action: `~ → Ctrl+V → Enter → ~`

### Method 2: Python Script File (Complex Operations)
Agent writes a `.py` script to `Content/Python/`, then copies execution command to clipboard.

```powershell
# Agent writes script to disk, then:
powershell -Command "Set-Clipboard -Value 'py \"D:/documents/Unreal Projects/game_maze/Content/Python/my_script.py\"'"
```

### Method 3: UE Bridge Functions (Chained Operations)
Load the bridge once, then call functions directly from console:

```
py "D:/documents/Unreal Projects/game_maze/Content/Python/ue_bridge.py"
```

Then subsequent commands:
```
py ue_bridge.diagnostic()
py ue_bridge.preset_cinematic()
py ue_bridge.rebuild_nav()
py ue_bridge.save_level()
```

## Command Templates

### Setup & Diagnostics
| Clipboard Command | Purpose |
|-------------------|---------|
| `py "D:/.../ue_bridge.py"` | Load bridge functions |
| `py ue_bridge.diagnostic()` | Check all maze actors present |
| `py ue_bridge.list_actors()` | List every actor in level |
| `py ue_bridge.save_level()` | Save current level |

### Rendering
| Clipboard Command | Purpose |
|-------------------|---------|
| `py ue_bridge.preset_cinematic()` | Max quality settings |
| `py ue_bridge.preset_performance()` | Optimized settings |
| `py ue_bridge.preset_debug()` | Show FPS, collision, NavMesh |
| `py ue_bridge.preset_debug_off()` | Toggle debug off |

### Navigation
| Clipboard Command | Purpose |
|-------------------|---------|
| `py ue_bridge.rebuild_nav()` | Rebuild NavMesh |
| `py ue_bridge.show_nav()` | Toggle NavMesh overlay |

### Direct Console Commands
| Clipboard Command | Purpose |
|-------------------|---------|
| `py ue_bridge.exec_cmd("slomo 0.5")` | Half-speed |
| `py ue_bridge.exec_cmd("ghost")` | No-clip mode |
| `py ue_bridge.exec_cmd("god")` | Invincibility |
| `py ue_bridge.exec_cmd("HighResShot 4")` | 4x screenshot |

## Shortcut Sequences (Post-Command)

After pasting commands, agents should instruct these shortcut sequences:

### Quick Test Cycle
```
~ → Ctrl+V → Enter → ~     (execute command)
Alt + P                      (play)
...test...
Esc                          (stop)
```

### Visual Inspection Sweep
```
~ → Ctrl+V → Enter → ~     (apply settings)
Alt + J                      (top-down view)
Home                         (zoom to fit)
F9                           (screenshot)
Alt + G                      (back to perspective)
```

### View Mode Cycle
```
Alt + 4    (Lit)
Alt + 5    (Detail Lighting)
Alt + 6    (Lighting Only)
Alt + 2    (Wireframe)
Alt + 4    (back to Lit)
```

## Error Recovery
| Problem | Solution |
|---------|----------|
| `py` not recognized | Enable Python plugin, restart editor |
| `ue_bridge` not found | Re-run: `py "D:/.../ue_bridge.py"` |
| Console won't open | Check keyboard layout; rebind `~` in DefaultEngine.ini |
| Command has no effect | Add `FlushRenderingCommands` after rendering changes |

## Outputs
- Console command execution results in Output Log
- Python function return values logged via `unreal.log()`
- Screenshots saved to `Saved/Screenshots/` when using `F9` or `HighResShot`

## References
- `docs/clipboard_console_automation.md` — 12 scenario walkthroughs
- `docs/agent_shortcut_reference.md` — per-agent shortcut maps
- `docs/console_commands_reference.md` — full command catalog
- `shortcut_keys_with_code_collaboration.md` — source methodology
