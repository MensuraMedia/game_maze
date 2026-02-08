# Unreal Engine Interface Reference

Shared knowledge base for all agents. Covers every method of interfacing with the
UE5 Editor on the local Windows 11 workstation.

> **UE5 Project:** `D:\documents\Unreal Projects\game_maze\game_maze.uproject`

---

## 1. Programmatic Interfaces

### 1.1 Python Editor Scripting

**Enable:** Edit > Plugins > Scripting > "Python Editor Script Plugin" → restart editor

**Access:** Window > Developer Tools > Output Log → Python tab, or run scripts from disk.

**Common APIs:**

```python
import unreal

# --- Actor Spawning ---
editor_lib = unreal.EditorLevelLibrary
world = editor_lib.get_editor_world()

# Spawn a static mesh actor at location
location = unreal.Vector(0, 0, 0)
rotation = unreal.Rotator(0, 0, 0)
actor = editor_lib.spawn_actor_from_class(
    unreal.StaticMeshActor, location, rotation
)

# --- Asset Management ---
asset_tools = unreal.AssetToolsHelpers.get_asset_tools()
asset_lib = unreal.EditorAssetLibrary

# Load an asset
mesh = unreal.EditorAssetLibrary.load_asset("/Game/Meshes/SM_Wall_01")

# List assets in a directory
assets = asset_lib.list_assets("/Game/Meshes/", recursive=True)

# --- Level Operations ---
editor_lib.save_current_level()
editor_lib.load_level("/Game/Maps/MazeLevel")

# --- Bulk Operations ---
# Import assets from disk
import_task = unreal.AssetImportTask()
import_task.filename = "D:/assets/wall.fbx"
import_task.destination_path = "/Game/Meshes"
import_task.automated = True
asset_tools.import_asset_tasks([import_task])
```

**Maze-Specific Examples:**

```python
# Spawn a grid of wall actors for maze generation
def spawn_maze_walls(grid, cell_size=200.0, wall_height=300.0):
    wall_mesh = unreal.EditorAssetLibrary.load_asset("/Game/Meshes/SM_Wall_01")
    for row in range(len(grid)):
        for col in range(len(grid[0])):
            cell = grid[row][col]
            base_x = col * cell_size
            base_y = row * cell_size
            if cell.north_wall:
                loc = unreal.Vector(base_x, base_y, wall_height / 2)
                actor = unreal.EditorLevelLibrary.spawn_actor_from_class(
                    unreal.StaticMeshActor, loc, unreal.Rotator(0, 0, 0)
                )
                actor.static_mesh_component.set_static_mesh(wall_mesh)
            # ... repeat for south, east, west walls

# Query all actors in the level
all_actors = unreal.EditorLevelLibrary.get_all_level_actors()
maze_actors = [a for a in all_actors if "Wall" in a.get_name()]
```

**Script Execution Methods:**
- UE Python console: `py "path/to/script.py"`
- Startup scripts: Place in `game_maze/Content/Python/` (auto-discovered)
- Command line: `UE5Editor.exe game_maze.uproject -ExecutePythonScript="script.py"`

---

### 1.2 C++ APIs

**Build Environment:** Visual Studio 2022 with UE5 workload

**Key Macros:**
| Macro | Purpose | Example |
|-------|---------|---------|
| `UCLASS()` | Declare a UE class | `UCLASS(Blueprintable) class AMazeGenerator : public AActor` |
| `UPROPERTY()` | Expose variable to editor | `UPROPERTY(EditAnywhere, Category="Maze") int32 Rows = 10;` |
| `UFUNCTION()` | Expose function | `UFUNCTION(BlueprintCallable) void GenerateMaze();` |
| `USTRUCT()` | Declare a data struct | `USTRUCT(BlueprintType) struct FMazeCell { ... };` |

**Gameplay Framework Classes:**
| Class | Purpose |
|-------|---------|
| `AGameModeBase` | Game rules, spawn logic |
| `AGameStateBase` | Shared game state (timer, score) |
| `APlayerState` | Per-player state |
| `ACharacter` | Player character with movement |
| `APlayerController` | Input handling, HUD ownership |

**Hot Reload:** Compile in VS2022 → Ctrl+Alt+F11 in UE Editor (or auto-detected)

**Module Setup:**
```
// game_maze.Build.cs
PublicDependencyModuleNames.AddRange(new string[] {
    "Core", "CoreUObject", "Engine", "InputCore",
    "EnhancedInput", "NavigationSystem", "AIModule", "UMG"
});
```

---

### 1.3 Blueprints Visual Scripting

**Access:** Content Browser > Right-Click > Blueprint Class

**Key Node Categories:**
- **Flow Control:** Branch, ForLoop, Sequence, Gate, DoOnce
- **Events:** BeginPlay, Tick, OnComponentBeginOverlap, InputAction
- **Timelines:** Animation curves for smooth transitions (light flicker, item bob)
- **Casting:** CastTo for type-safe access between Blueprints

**Blueprint-C++ Bridge:**
- C++ functions marked `UFUNCTION(BlueprintCallable)` appear as Blueprint nodes
- C++ events marked `UFUNCTION(BlueprintImplementableEvent)` can be overridden in BP
- Blueprint Function Libraries: static utility functions callable from any Blueprint

**Best Practices:**
- Use comment boxes (C key) to group logic
- Collapse repeated logic into functions/macros
- Avoid Tick when event-driven alternatives exist
- Expose tunable values via `UPROPERTY(EditAnywhere)`

---

### 1.4 Commandlets & Automation

**Run from command line:**
```cmd
"C:\Program Files\Epic Games\UE_5.x\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" ^
  "D:\documents\Unreal Projects\game_maze\game_maze.uproject" ^
  -run=MyCommandlet
```

**UE Automation Framework:**
```cpp
// Custom automation test
IMPLEMENT_SIMPLE_AUTOMATION_TEST(FMazeSolvabilityTest,
    "MazeGame.Maze.Solvability",
    EAutomationTestFlags::EditorContext | EAutomationTestFlags::ProductFilter)

bool FMazeSolvabilityTest::RunTest(const FString& Parameters)
{
    // Generate maze, validate path exists from start to exit
    return bPathFound;
}
```

**Run tests:** Session Frontend > Automation tab, or CLI: `-ExecCmds="Automation RunTests MazeGame"`

---

### 1.5 Remote Control API

**Enable:** Edit > Plugins > "Remote Control API" → restart

**HTTP Endpoints (default port 30010):**
```
GET  http://localhost:30010/remote/info
POST http://localhost:30010/remote/object/call
POST http://localhost:30010/remote/object/property
```

**Example — Set actor property via HTTP:**
```json
POST http://localhost:30010/remote/object/property
{
  "objectPath": "/Game/Maps/MazeLevel.MazeLevel:PersistentLevel.MazeGenerator_1",
  "propertyName": "Rows",
  "propertyValue": { "Rows": 20 }
}
```

**Example — Call function remotely:**
```json
POST http://localhost:30010/remote/object/call
{
  "objectPath": "/Game/Maps/MazeLevel.MazeLevel:PersistentLevel.MazeGenerator_1",
  "functionName": "GenerateMaze"
}
```

**Use Cases:**
- External Python scripts controlling the editor
- AI agent sending commands to UE via REST
- Dashboard monitoring game state in real time

---

### 1.6 Plugin APIs

**Relevant Plugins:**
| Plugin | Purpose | Enable Path |
|--------|---------|-------------|
| Python Editor Script | Python automation | Edit > Plugins > Scripting |
| Remote Control API | HTTP/WebSocket control | Edit > Plugins > Virtual Production |
| Editor Scripting Utilities | Extended editor Python API | Edit > Plugins > Scripting |
| Procedural Content Generation | PCG framework | Edit > Plugins > Procedural |
| AI Module | Behavior Trees, EQS | Built-in (always available) |

**Custom Plugin Creation:**
- Tools menu > New Plugin > Editor Standalone Window (for custom tools)
- Expose functionality via `IModuleInterface` and `FModuleManager`

---

## 2. Resource Files

### 2.1 Asset Files

| Extension | Type | Description |
|-----------|------|-------------|
| `.uasset` | Content | Blueprints, meshes, materials, textures, sounds |
| `.umap` | Level | World/map files |
| `.uproject` | Project | Project configuration and module list |

**Project File:** `D:\documents\Unreal Projects\game_maze\game_maze.uproject`

**Programmatic Access:**
```python
# Python: List all assets
assets = unreal.EditorAssetLibrary.list_assets("/Game/", recursive=True)

# Python: Duplicate an asset
unreal.EditorAssetLibrary.duplicate_asset(
    "/Game/Meshes/SM_Wall_01", "/Game/Meshes/SM_Wall_02"
)

# Python: Delete an asset
unreal.EditorAssetLibrary.delete_asset("/Game/Meshes/SM_Wall_Old")
```

### 2.2 Config Files

**Location:** `D:\documents\Unreal Projects\game_maze\Config\`

| File | Controls |
|------|----------|
| `DefaultEngine.ini` | Rendering, physics, navigation settings |
| `DefaultGame.ini` | Project name, version, packaging |
| `DefaultInput.ini` | Input bindings (legacy system) |
| `DefaultEditor.ini` | Editor preferences |
| `DefaultEditorPerProjectUserSettings.ini` | Per-user editor settings |

**Edit programmatically:**
```python
# Python: Read/write config
config = unreal.ConfigUtilities
# Or edit directly via file I/O since they're plain .ini files
```

### 2.3 Log Files

**Location:** `D:\documents\Unreal Projects\game_maze\Saved\Logs\`

- `game_maze.log` — Current session log
- Monitor in real-time: Output Log window in UE Editor
- Parse programmatically for error detection

---

## 3. Hotkey Reference

> **Full source:** `shortcut_keys_reference.md` (400+ shortcuts)
> **Per-agent summaries:** `docs/agent_shortcut_reference.md`
> **Customize:** Edit > Editor Preferences > Keyboard Shortcuts
> **Export:** Click "Export" in Keyboard Shortcuts to save as `.KeyBindings` file

### 3.1 Global / Common Editing

| Key | Action |
|-----|--------|
| `Ctrl + Z` | Undo |
| `Ctrl + Y` | Redo |
| `Ctrl + C / X / V` | Copy / Cut / Paste |
| `Ctrl + W` / `Ctrl + D` | Duplicate |
| `Delete` / `Backspace` | Delete selected |
| `Ctrl + A` | Select All |
| `F2` | Rename |
| `Ctrl + S` | Save |
| `Ctrl + Shift + S` | Save All |
| `Ctrl + Alt + S` | Save As... |
| `Ctrl + N` | New Level/Folder |
| `Ctrl + O` | Open Level |
| `Esc` | Deselect All / Cancel |

### 3.2 Viewport Navigation & Camera

| Key | Action |
|-----|--------|
| `RMB + WASD` | Move camera (GameStyle) |
| `RMB + QE` | Camera Up/Down |
| `RMB Drag` | Orbit / Pan |
| `Mouse Wheel` | Zoom |
| `Alt + LMB` | Orbit around selection |
| `Alt + RMB` | Dolly (zoom) |
| `RMB + Mouse Wheel` | Adjust camera speed |
| `Home` | Zoom to Fit |
| `F` | Focus on selected actor |

### 3.3 Transformation Tools

| Key | Action |
|-----|--------|
| `Q` | Select Mode |
| `W` | Translate/Move |
| `E` | Rotate |
| `R` | Scale |
| `Space` | Cycle Tools (Q→W→E→R) |
| `Alt + Drag` | Duplicate + Transform |
| `` Ctrl + ` `` | Cycle Coord System (World/Local) |

### 3.4 Editor Modes

| Key | Action |
|-----|--------|
| `Shift + 1` | Place/Select |
| `Shift + 2` | Landscape |
| `Shift + 3` | Foliage |
| `Shift + 4` | Mesh Paint |
| `Shift + 5` | Modeling |
| `Shift + 6` | Fracture |
| `Shift + 7` | Brush Editing |
| `Shift + 8` | Animation |

### 3.5 View Modes

| Key | Action |
|-----|--------|
| `Alt + 1` | Value (default) |
| `Alt + 2` | Wireframe |
| `Alt + 3` | Unlit |
| `Alt + 4` | Lit |
| `Alt + 5` | Detail Lighting |
| `Alt + 6` | Lighting Only |
| `Alt + 7` | Light Complexity |
| `Alt + 8` | Shader Complexity |
| `Alt + 0` | Lightmap Density |

### 3.6 Camera Views & Bookmarks

| Key | Action |
|-----|--------|
| `Alt + G` | Perspective |
| `Alt + H` | Front |
| `Alt + J` | Top |
| `Alt + K` | Left |
| `Alt + Shift + J` | Bottom |
| `Alt + Shift + K` | Right |
| `Alt + Shift + H` | Back |
| `Ctrl + 0-9` | Set Bookmark |
| `0-9` | Jump to Bookmark |

### 3.7 Selection, Visibility & Snapping

| Key | Action |
|-----|--------|
| `Ctrl + Shift + A` | Select All of Class/Mesh |
| `Shift + E` | Select Matching Actors |
| `H` | Hide Selected |
| `Ctrl + H` | Unhide All |
| `T` | Translucent Selection |
| `G` | Toggle Game View |
| `End` | Snap to Floor |
| `Alt + End` | Snap Pivot to Floor |
| `Shift + End` | Snap Bounds to Floor |
| `Ctrl + End` | Snap to Grid |
| `[` / `]` | Decrease / Increase Grid Size |
| `Shift + [` / `]` | Decrease / Increase Rotation Snap |
| `V` | Vertex Snapping (hold + drag) |

### 3.8 Play, Simulate & Debug

| Key | Action |
|-----|--------|
| `Alt + P` | Play (PIE) |
| `Alt + S` | Simulate |
| `F8` | Possess Player / Eject |
| `Pause` | Pause |
| `Esc` | Stop PIE |
| `F9` | Screenshot |
| `F11` | Immersive Mode |
| `Shift + F11` | Fullscreen Editor |
| `Ctrl + Shift + H` | Toggle FPS |
| `Shift + L` | Toggle Stats |
| `~` (tilde) | Console |
| `Ctrl + Shift + ,` | GPU Profiler |

### 3.9 Content Browser & Assets

| Key | Action |
|-----|--------|
| `Ctrl + P` | Search Project |
| `Ctrl + B` | Browse to Asset |
| `Ctrl + E` | Edit Selected Asset |
| `Space` | Preview Asset |
| `Enter` | Open Asset/Folder |
| `Ctrl + Shift + N` | New Folder |
| `Alt + Shift + A` | Audit Assets |
| `Alt + Shift + R` | Reference Viewer |
| `Alt + Shift + M` | Size Map |

### 3.10 Blueprint Editor

| Key | Action |
|-----|--------|
| `F7` | Compile |
| `Ctrl + F` | Find in Blueprint |
| `Ctrl + Shift + F` | Find in All Blueprints |
| `Home` | Zoom to Fit |
| `RMB Drag` | Pan Graph |
| `Page Up/Dn` | Parent/Child Graph |
| `B + Click` | Branch |
| `S + Click` | Sequence |
| `D + Click` | Delay |
| `F + Click` | ForEach Loop |
| `C + Click` | Comment |
| `F9` | Toggle Breakpoint |
| `Ctrl + Shift + F9` | Clear All Breakpoints |
| `Alt + Click (pin)` | Break Link |

### 3.11 Material & Mesh Editors

| Key | Action |
|-----|--------|
| `I` | Toggle Environment |
| `O` | Toggle Floor |
| `P` | Toggle Post Process |
| `Ctrl + N` | Nanite Fallback (Static Mesh) |
| `Alt + N` | Toggle Normals |
| `Alt + S` | Toggle Sockets |

### 3.12 Modeling Mode (Shift + 5)

| Key | Action |
|-----|--------|
| `Enter` | Accept/Complete |
| `Esc` | Cancel |
| `A` | Toggle Gizmo |
| `[ / ]` | Brush Size +/- |
| `Ctrl + [ / ]` | Brush Strength +/- |
| `T` | Flip Selection |
| `E` | Extrude |
| `Q` | Push/Pull |

### 3.13 Build & Compilation

| Key | Action |
|-----|--------|
| `Ctrl + Alt + F11` | Hot Reload C++ (Live Coding) |
| `Ctrl + Alt + L` | Rebuild Lighting |
| `Build` menu | Full rebuild (geometry, lighting, navigation) |

**AutoHotkey Integration:** Agents may generate AHK scripts for complex sequences:
```ahk
; Example: Save, compile, and play
^+p::  ; Ctrl+Shift+P
    Send, ^s          ; Save
    Sleep, 500
    Send, ^!{F11}     ; Hot reload
    Sleep, 2000
    Send, !p          ; Play in editor
return
```

---

## 4. Console Commands

**Access:** Press `~` (tilde) in editor viewport or PIE

### 4.1 Performance & Debugging

| Command | Description |
|---------|-------------|
| `stat fps` | Show framerate |
| `stat unit` | Show frame time breakdown (game, render, GPU) |
| `stat memory` | Memory usage |
| `stat scenerendering` | Rendering stats |
| `stat navigation` | NavMesh stats |
| `profilegpu` | GPU profiler |
| `obj list` | List all UObjects |
| `dumpconsolecommands` | List all available console commands |

### 4.2 Navigation & AI

| Command | Description |
|---------|-------------|
| `RebuildNavigation` | Rebuild NavMesh |
| `show navigation` | Toggle NavMesh visualization |
| `ai.debug` | Toggle AI debugging overlays |

### 4.3 Level & World

| Command | Description |
|---------|-------------|
| `open <MapName>` | Load a level |
| `restartlevel` | Restart current level |
| `slomo <float>` | Time dilation (e.g., `slomo 0.5` for half speed) |

### 4.4 Rendering

| Command | Description |
|---------|-------------|
| `show <flag>` | Toggle visibility (e.g., `show collision`, `show bounds`) |
| `r.Lumen.Enabled 1/0` | Toggle Lumen GI |
| `r.VolumetricFog 1/0` | Toggle volumetric fog |
| `HighResShot <multiplier>` | Take high-res screenshot |

### 4.5 Testing (Cheat Manager)

| Command | Description |
|---------|-------------|
| `god` | Toggle invincibility |
| `fly` | Toggle fly mode |
| `ghost` | No-clip mode |
| `teleport` | Teleport to crosshair |

---

## 5. External Tool Integration

### 5.1 Visual Studio 2022

- Generate project files: Right-click `.uproject` > "Generate Visual Studio project files"
- Or via CLI: `UnrealBuildTool.exe -projectfiles -project="game_maze.uproject" -game -engine`
- Attach debugger: Debug > Attach to Process > `UnrealEditor.exe`
- Breakpoints work in C++ gameplay code

### 5.2 Git Integration

**From project root:**
```cmd
cd "D:\documents\Unreal Projects\game_maze"
git init
git remote add origin https://github.com/MensuraMedia/game_maze.git
```

**Recommended `.gitignore` for UE5:**
```
Binaries/
DerivedDataCache/
Intermediate/
Saved/
.vs/
*.sln
*.vcxproj*
```

**Git LFS** (recommended for binary assets):
```cmd
git lfs install
git lfs track "*.uasset" "*.umap" "*.png" "*.wav" "*.fbx"
```

### 5.3 AutoHotkey (Windows Automation)

For automating repetitive editor workflows:
```ahk
; Focus UE Editor window and execute a sequence
WinActivate, ahk_exe UnrealEditor.exe
Send, {~}                     ; Open console
Sleep, 200
Send, RebuildNavigation{Enter} ; Run command
Send, {~}                     ; Close console
```

### 5.4 File Watchers

Monitor asset changes and trigger actions:
```python
# Python file watcher for Content directory
import time, os
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler

class AssetHandler(FileSystemEventHandler):
    def on_modified(self, event):
        if event.src_path.endswith('.uasset'):
            print(f"Asset modified: {event.src_path}")
            # Trigger reimport or validation

observer = Observer()
observer.schedule(AssetHandler(),
    r"D:\documents\Unreal Projects\game_maze\Content", recursive=True)
observer.start()
```

---

## 6. AI-Specific Integration

### 6.1 MCP (Model Context Protocol) Integration

Agents can use MCP servers to bridge Claude and the UE Editor:
- **UnrealMCP / UE5-MCP**: Plugin exposing editor actions as MCP tool calls
- Allows natural language → editor action translation
- Example: "Place a point light at the player start" → Python script execution

### 6.2 Remote Control + Python Bridge

Architecture for AI-driven editor control:
```
Claude Agent → Python Script → Remote Control API → UE Editor
                    ↓
              File System (configs, scripts, assets)
```

### 6.3 Discovery Methods

When a needed feature or API is unknown:
1. **Console:** Run `dumpconsolecommands` to list all commands
2. **Python:** `dir(unreal)` to list available Python modules
3. **Docs:** Check local UE docs (Help > Documentation in editor)
4. **grep.app:** Search https://grep.app/ for UE-related open-source code
5. **Claude:** Prompt for API suggestions based on UE version and requirements
6. **Reflection:** Use `unreal.find_class()` and `get_editor_property()` for runtime discovery

---

## 7. Agent-Specific Quick Reference

| Agent | Primary Interfaces | Key Shortcuts |
|-------|--------------------|---------------|
| 00 Conductor | Integration testing, PIE, console | `Alt+P`, `stat fps`, `stat unit` |
| 01 Team Lead | Build, review, asset audit | `Ctrl+Alt+F11`, `Alt+Shift+R` |
| 02 Game Director | Play-testing, design tuning | `Alt+P`, `slomo`, `ghost`, `god` |
| 03 Level Designer | Actor placement, grid snap, PCG | `W/E/R`, `End`, `[/]`, `Alt+J` |
| 04 Character Controller | Movement debug, collision | `show collision`, `showdebug MOVEMENT` |
| 05 Blueprint Agent | Blueprint Editor, node creation | `F7`, `B+Click`, `F9`, `C+Click` |
| 06 C++ Programmer | VS2022, Hot Reload, profiling | `Ctrl+Alt+F11`, `profilegpu`, `stat` |
| 07 Environment Artist | Content Browser, Material Editor | `Ctrl+P`, `Alt+N`, `Shift+5` |
| 08 AI & Navigation | NavMesh, AI debug | `RebuildNavigation`, `show navigation` |
| 09 Lighting | View modes, Lumen, fog | `Alt+4/5/6`, `r.Lumen.*`, `r.VolumetricFog` |
| 10 Audio | Sound debug, spatial audio | `au.Debug.Sounds`, `stat audio` |
| 11 UI/UX | Widget editor, HUD debug | `F7`, `G`, `show hud`, `stat slate` |

> **Detailed per-agent shortcuts:** See `docs/agent_shortcut_reference.md`
> **Console commands by domain:** See `docs/console_commands_reference.md`

---

*Maintained collaboratively by all agents. Version-controlled via Git.*
*Update by appending discoveries as new sections or expanding existing ones.*
*Last updated: 2026-02-07*
