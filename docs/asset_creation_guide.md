# Asset Creation Guide — Maze Explorer

**Version:** 1.0
**Status:** Active
**Author:** Agent 07 (Environment Artist)
**Engine:** Unreal Engine 5.7
**Related:** `docs/GDD.md` Section 4 (Maze System), Section 7 (Visual Style)

---

## Table of Contents

1. [Overview](#1-overview)
2. [Running the Python Script](#2-running-the-python-script)
3. [Folder Structure](#3-folder-structure)
4. [Mesh Specifications](#4-mesh-specifications)
5. [Material Setup](#5-material-setup)
6. [Material Instances](#6-material-instances)
7. [Configuration Data](#7-configuration-data)
8. [Manual Steps](#8-manual-steps-that-cannot-be-automated)
9. [Asset Naming Conventions](#9-asset-naming-conventions)
10. [Troubleshooting](#10-troubleshooting)

---

## 1. Overview

This guide documents all modular environment assets for the Maze Explorer game. The maze is built from three tile types — wall, floor, and ceiling — snapped to a grid defined by the GDD:

| Parameter | Value | Source |
|-----------|-------|--------|
| Cell Size | 200 x 200 UU | GDD 4.1 |
| Wall Height | 300 UU | GDD 4.1 |
| Wall Thickness | 20 UU | GDD 4.1 |
| Floor Thickness | 10 UU | Script default |
| Ceiling Thickness | 10 UU | Script default |
| Default Grid | 10 x 10 | GDD 4.1 |
| Max Grid | 50 x 50 | GDD 4.1 |

A Python editor script automates the creation of folder structure, materials, material instances, and configuration data. Static meshes must be created manually (see Section 8).

---

## 2. Running the Python Script

### Prerequisites

1. **Enable plugins** in the UE5 Editor (Edit > Plugins):
   - Python Editor Script Plugin (under Scripting)
   - Editor Scripting Utilities (under Scripting)
2. **Restart the editor** after enabling plugins.

### Execution Methods

**Method A — Output Log Python Tab (recommended):**

```
Window > Developer Tools > Output Log
```

Switch to the Python tab at the bottom, then type:

```python
py "D:/documents/Unreal Projects/game_maze/Content/Python/create_maze_assets.py"
```

**Method B — Import as Module:**

In the Python tab:

```python
import sys
sys.path.insert(0, r"D:\documents\Unreal Projects\game_maze\Content\Python")
import create_maze_assets
create_maze_assets.main()
```

**Method C — Command Line (headless):**

```cmd
"C:\Program Files\Epic Games\UE_5.7\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" ^
  "D:\documents\Unreal Projects\game_maze\game_maze.uproject" ^
  -ExecutePythonScript="D:/documents/Unreal Projects/game_maze/Content/Python/create_maze_assets.py"
```

### Re-running the Script

The script is idempotent. Running it multiple times is safe — existing assets are detected and skipped. Check the Output Log for `[MazeAssets]` prefixed messages.

### Running Individual Steps

Each step can be called independently after importing the module:

```python
import create_maze_assets as ma

ma.create_folder_structure()          # Step 1
ma.create_all_master_materials()      # Step 2
ma.create_all_material_instances()    # Step 3
ma.create_maze_config_data_asset()    # Step 4
ma.print_mesh_creation_instructions() # Step 5
ma.verify_assets()                    # Step 6
```

---

## 3. Folder Structure

Created automatically by the script:

```
/Game/Maze/
    Meshes/                         Static mesh assets
        SM_Wall_200x20x300          (manual)
        SM_Floor_200x200x10         (manual)
        SM_Ceiling_200x200x10       (manual)
    Materials/                      Master materials
        M_Wall_Master               (automated)
        M_Floor_Master              (automated)
        M_Ceiling_Master            (automated)
        Instances/                  Material instances
            MI_Wall_Stone           (automated)
            MI_Wall_Brick           (automated)
            MI_Floor_Stone          (automated)
            MI_Floor_Metal          (automated)
            MI_Ceiling_Stone        (automated)
    Blueprints/                     Blueprint actors (future)
    Data/                           Configuration data
        MazeConfig.json             (automated)
        MazeConfig_README.txt       (automated)
```

---

## 4. Mesh Specifications

### 4.1 SM_Wall_200x20x300

| Property | Value |
|----------|-------|
| **Asset Path** | `/Game/Maze/Meshes/SM_Wall_200x20x300` |
| **Dimensions** | 200 x 20 x 300 UU (Width x Depth x Height) |
| **Pivot Point** | Center-Bottom (X=0, Y=0, Z=0 at bottom face center) |
| **UV Mapping** | Planar per face, 1 UV channel, normalized 0-1 |
| **Collision** | Simple box collision |
| **Nanite** | Enabled |
| **Purpose** | One wall segment spanning one cell edge |

**Dimension rationale:**
- Width (200 UU) matches `CellSize` so one wall spans exactly one cell edge.
- Depth (20 UU) matches `WallThickness` from the GDD.
- Height (300 UU) matches `WallHeight`, taller than the player eye height (170 UU).

**LOD policy (if Nanite is unavailable):**

| LOD | Triangle Budget | Screen Size Threshold |
|-----|----------------|----------------------|
| LOD0 | Full (24 tris for a box) | 1.0 |
| LOD1 | 50% | 0.5 |
| LOD2 | 25% | 0.25 |

With Nanite enabled, manual LODs are unnecessary.

### 4.2 SM_Floor_200x200x10

| Property | Value |
|----------|-------|
| **Asset Path** | `/Game/Maze/Meshes/SM_Floor_200x200x10` |
| **Dimensions** | 200 x 200 x 10 UU (Width x Depth x Height) |
| **Pivot Point** | Center-Bottom (X=0, Y=0, Z=0 at bottom face center) |
| **UV Mapping** | Planar from top, 1 UV channel |
| **Collision** | Simple box collision |
| **Nanite** | Enabled |
| **Purpose** | One floor tile covering one maze cell |

**Placement:** At Z=0 for each cell. The maze generator places floor tiles at `(col * CellSize, row * CellSize, 0)`.

### 4.3 SM_Ceiling_200x200x10

| Property | Value |
|----------|-------|
| **Asset Path** | `/Game/Maze/Meshes/SM_Ceiling_200x200x10` |
| **Dimensions** | 200 x 200 x 10 UU (Width x Depth x Height) |
| **Pivot Point** | Center-Bottom (X=0, Y=0, Z=0 at bottom face center) |
| **UV Mapping** | Planar from bottom, 1 UV channel |
| **Collision** | Simple box collision |
| **Nanite** | Enabled |
| **Purpose** | One ceiling tile covering one maze cell |

**Placement:** At Z = `WallHeight` (300 UU). The ceiling sits on top of the walls.

### 4.4 Pivot Point Standard

All meshes use **center-bottom** pivot:

```
  Top View (X-Y plane)        Side View (X-Z plane)
  ┌──────────────────┐        ┌──────────────────┐  Z = Height
  │                  │        │                  │
  │        *         │        │                  │
  │     (pivot)      │        │                  │
  │                  │        │                  │
  └──────────────────┘        └────────*─────────┘  Z = 0 (pivot)
                                    (pivot)
```

This convention means:
- Placing an actor at `(X, Y, 0)` puts the mesh bottom at ground level.
- Wall meshes extend upward from Z=0 to Z=300.
- No offset calculations are needed during maze generation.

---

## 5. Material Setup

### 5.1 Master Material Architecture

Each master material exposes the same set of parameters, enabling uniform control across all material instances:

```
┌─────────────────────────────────────────────┐
│            M_*_Master Material               │
│                                             │
│  [VectorParam: BaseColor] ──────> Base Color│
│  [ScalarParam: Roughness] ─────> Roughness  │
│  [ScalarParam: Metallic]  ─────> Metallic   │
│  [Tex2DParam:  NormalMap] ──┐               │
│  [ScalarParam: NormalStrength]─> Normal     │
│  [ScalarParam: TilingU]   ──┐               │
│  [ScalarParam: TilingV]   ──┴──> UV Tiling  │
│  [TexCoord Node]          ──────> UVs       │
│                                             │
│  BlendMode: Opaque                          │
│  TwoSided:  false                           │
└─────────────────────────────────────────────┘
```

### 5.2 M_Wall_Master

| Parameter | Default Value | Description |
|-----------|--------------|-------------|
| BaseColor | (0.35, 0.35, 0.38, 1.0) | Cool grey with subtle blue tint |
| Roughness | 0.85 | Rough stone surface |
| Metallic | 0.0 | Non-metallic stone |
| NormalMap | None (empty) | Assign stone normal map in instances |
| NormalStrength | 1.0 | Normal map intensity multiplier |
| TilingU | 1.0 | Horizontal UV repeat |
| TilingV | 1.0 | Vertical UV repeat |

### 5.3 M_Floor_Master

| Parameter | Default Value | Description |
|-----------|--------------|-------------|
| BaseColor | (0.30, 0.30, 0.32, 1.0) | Slightly darker grey flagstone |
| Roughness | 0.80 | Worn but slightly smoother than walls |
| Metallic | 0.0 | Non-metallic |
| NormalMap | None | Assign flagstone normal map in instances |
| NormalStrength | 1.0 | Normal map intensity |
| TilingU | 1.0 | Horizontal UV repeat |
| TilingV | 1.0 | Vertical UV repeat |

### 5.4 M_Ceiling_Master

| Parameter | Default Value | Description |
|-----------|--------------|-------------|
| BaseColor | (0.22, 0.22, 0.25, 1.0) | Darkest grey, recedes visually |
| Roughness | 0.90 | Very rough, absorbs light |
| Metallic | 0.0 | Non-metallic |
| NormalMap | None | Assign smooth stone normal map in instances |
| NormalStrength | 1.0 | Normal map intensity |
| TilingU | 1.0 | Horizontal UV repeat |
| TilingV | 1.0 | Vertical UV repeat |

---

## 6. Material Instances

### 6.1 Instance Reference Table

| Instance | Parent | BaseColor | Roughness | Metallic | TilingU | TilingV | NormalStrength |
|----------|--------|-----------|-----------|----------|---------|---------|----------------|
| MI_Wall_Stone | M_Wall_Master | (0.38, 0.37, 0.40) | 0.88 | 0.0 | 2.0 | 3.0 | 1.2 |
| MI_Wall_Brick | M_Wall_Master | (0.45, 0.35, 0.28) | 0.78 | 0.0 | 4.0 | 6.0 | 1.0 |
| MI_Floor_Stone | M_Floor_Master | (0.32, 0.31, 0.33) | 0.82 | 0.0 | 2.0 | 2.0 | 0.9 |
| MI_Floor_Metal | M_Floor_Master | (0.25, 0.22, 0.20) | 0.60 | 0.75 | 4.0 | 4.0 | 1.5 |
| MI_Ceiling_Stone | M_Ceiling_Master | (0.20, 0.20, 0.23) | 0.92 | 0.0 | 2.0 | 2.0 | 0.6 |

### 6.2 Instance Descriptions

**MI_Wall_Stone** — Primary wall material. Rough-cut stone blocks with cool grey tones and strong normal detail. Used for the majority of maze walls. Tiling is taller (3.0 V) to match the wall's 300 UU height.

**MI_Wall_Brick** — Variation wall material. Warmer brick-like tones with tighter tiling for a smaller brick pattern. Used sparingly to break visual monotony in certain maze sections.

**MI_Floor_Stone** — Primary floor material. Worn flagstone with subtle cracks. Even tiling (2x2) for square floor tiles.

**MI_Floor_Metal** — Variation floor material. Dark rusted metal grate, metallic at 0.75, lower roughness for reflections. Used rarely in special sections per the GDD.

**MI_Ceiling_Stone** — Primary ceiling material. Very dark smooth stone. Low normal strength (0.6) since ceilings are less visually prominent.

### 6.3 Adding Normal Map Textures

Once normal map textures are imported:

1. Open the material instance in the Material Instance Editor.
2. Under "Texture Parameter Values", check the override box for `NormalMap`.
3. Assign the imported texture.
4. Adjust `NormalStrength` if needed.

Or via Python:

```python
import unreal
mel = unreal.MaterialEditingLibrary
mi = unreal.EditorAssetLibrary.load_asset(
    "/Game/Maze/Materials/Instances/MI_Wall_Stone"
)
normal_tex = unreal.EditorAssetLibrary.load_asset(
    "/Game/Maze/Textures/T_Stone_N"
)
mel.set_material_instance_texture_parameter_value(mi, "NormalMap", normal_tex)
unreal.EditorAssetLibrary.save_asset(
    "/Game/Maze/Materials/Instances/MI_Wall_Stone"
)
```

---

## 7. Configuration Data

### 7.1 MazeConfig.json

Located at: `Content/Maze/Data/MazeConfig.json`

This JSON file contains all default maze parameters, the difficulty progression table, asset path references, and mesh dimension specs. It is generated automatically by the Python script.

**Structure:**

```json
{
    "MazeDefaults": {
        "Rows": 10,
        "Cols": 10,
        "CellSize": 200.0,
        "WallHeight": 300.0,
        "WallThickness": 20.0,
        "FloorThickness": 10.0,
        "CeilingThickness": 10.0,
        "Seed": -1,
        "NumCollectibles": 5,
        "OpenWallPercent": 0.1
    },
    "DifficultyLevels": [ ... ],
    "AssetPaths": { ... },
    "MeshSpecs": { ... }
}
```

### 7.2 Loading in C++

```cpp
FString JsonStr;
FString ConfigPath = FPaths::ProjectContentDir() / TEXT("Maze/Data/MazeConfig.json");
if (FFileHelper::LoadFileToString(JsonStr, *ConfigPath))
{
    TSharedPtr<FJsonObject> JsonObj;
    TSharedRef<TJsonReader<>> Reader = TJsonReaderFactory<>::Create(JsonStr);
    if (FJsonSerializer::Deserialize(Reader, JsonObj))
    {
        TSharedPtr<FJsonObject> Defaults = JsonObj->GetObjectField("MazeDefaults");
        int32 Rows = Defaults->GetIntegerField("Rows");
        float CellSize = Defaults->GetNumberField("CellSize");
        // ... etc
    }
}
```

### 7.3 Loading in Python (inside UE)

```python
import json, os, unreal
config_path = os.path.join(
    unreal.Paths.project_content_dir(), "Maze", "Data", "MazeConfig.json"
)
with open(config_path) as f:
    config = json.load(f)

rows = config["MazeDefaults"]["Rows"]
cell_size = config["MazeDefaults"]["CellSize"]
```

---

## 8. Manual Steps That Cannot Be Automated

The UE5 Python API cannot generate StaticMesh geometry. The following steps must be performed manually in the editor.

### 8.1 Creating Static Meshes

**Recommended approach — UE5 Modeling Mode:**

1. Switch to Modeling Mode (Shift+5 or use the mode selector in the top-left toolbar).
2. In the Modeling tools panel, click **Shapes** then **Box**.
3. Draw the box in the viewport, then set exact dimensions in the Details panel.
4. Right-click the actor > **Convert to Static Mesh**.
5. In the save dialog, navigate to `/Game/Maze/Meshes/` and name it per the naming convention.

**Repeat for each mesh:**

| Mesh | Box Width | Box Depth | Box Height |
|------|-----------|-----------|------------|
| SM_Wall_200x20x300 | 200 | 20 | 300 |
| SM_Floor_200x200x10 | 200 | 200 | 10 |
| SM_Ceiling_200x200x10 | 200 | 200 | 10 |

### 8.2 Setting the Pivot Point

After converting to a static mesh:

1. Open the Static Mesh Editor by double-clicking the asset.
2. Verify the pivot is at center-bottom. If not:
   - In the Details panel, set **Pivot Offset** so the pivot sits at the bottom-center.
   - Or re-create the mesh in Modeling Mode with the correct origin placement.

### 8.3 Enabling Nanite

For each static mesh:

1. Open the Static Mesh Editor.
2. In the Details panel, find the **Nanite** section.
3. Check **Enable Nanite Support**.
4. Click **Apply Changes**.

### 8.4 Setting Collision

For each static mesh:

1. In the Static Mesh Editor, go to the **Collision** menu.
2. Select **Add Box Simplified Collision**.
3. Verify the collision box matches the mesh bounds.

### 8.5 Assigning Default Materials

For each static mesh:

1. In the Static Mesh Editor, find the **Material Slots** section.
2. Assign the appropriate material instance:
   - SM_Wall: MI_Wall_Stone
   - SM_Floor: MI_Floor_Stone
   - SM_Ceiling: MI_Ceiling_Stone

### 8.6 Verification

After completing all manual steps, run the verification function:

```python
import create_maze_assets
create_maze_assets.verify_assets()
```

All items should report `[FOUND]`.

---

## 9. Asset Naming Conventions

### 9.1 Prefix Table

| Asset Type | Prefix | Example |
|------------|--------|---------|
| Static Mesh | `SM_` | `SM_Wall_200x20x300` |
| Material (Master) | `M_` | `M_Wall_Master` |
| Material Instance | `MI_` | `MI_Wall_Stone` |
| Texture | `T_` | `T_Stone_D` (diffuse), `T_Stone_N` (normal) |
| Blueprint | `BP_` | `BP_MazeWall` |
| Data Table | `DT_` | `DT_MazeConfig` |
| Widget Blueprint | `WBP_` | `WBP_HUD` |
| Niagara System | `NS_` | `NS_CollectiblePickup` |
| Sound Cue | `SC_` | `SC_Footstep_Stone` |
| MetaSound | `MS_` | `MS_AmbientDrone` |

### 9.2 Mesh Naming Pattern

Static meshes include dimensions in the name for clarity:

```
SM_{Type}_{WidthX}x{DepthY}x{HeightZ}
```

Examples:
- `SM_Wall_200x20x300`
- `SM_Floor_200x200x10`
- `SM_Ceiling_200x200x10`

### 9.3 Material Naming Pattern

**Master materials:**
```
M_{Surface}_Master
```

**Material instances:**
```
MI_{Surface}_{Variant}
```

Examples:
- `M_Wall_Master` (parent for all wall materials)
- `MI_Wall_Stone` (instance: stone variant)
- `MI_Wall_Brick` (instance: brick variant)

### 9.4 Texture Naming Pattern

```
T_{Surface}_{Type}
```

Where `{Type}` is one of:
- `D` — Diffuse / Base Color
- `N` — Normal Map
- `R` — Roughness
- `M` — Metallic
- `AO` — Ambient Occlusion
- `E` — Emissive

Examples:
- `T_Stone_D` — Stone diffuse
- `T_Stone_N` — Stone normal
- `T_Brick_D` — Brick diffuse
- `T_Metal_R` — Metal roughness

### 9.5 Directory Conventions

```
/Game/Maze/
    Meshes/          All static meshes for the maze environment
    Materials/       Master materials only
        Instances/   All material instances
    Textures/        All texture assets (when imported)
    Blueprints/      Blueprint actors and components
    Data/            Configuration files and data tables
```

---

## 10. Troubleshooting

### "ModuleNotFoundError: No module named 'unreal'"

The script must be run inside the UE5 Editor Python environment. It cannot be run from a standalone Python installation. Use one of the execution methods in Section 2.

### "Plugin not found" or missing MaterialEditingLibrary

Ensure both plugins are enabled:
- Python Editor Script Plugin
- Editor Scripting Utilities

Restart the editor after enabling.

### Materials appear black or incorrect

1. Verify the material compiled successfully (open in Material Editor, check for errors).
2. Run `unreal.MaterialEditingLibrary.recompile_material(material)` on the asset.
3. Check that the material instance parent is correctly set.

### Script reports assets as MISSING after running

- **Materials/Instances**: These should be created automatically. Check the Output Log for error messages.
- **Static Meshes**: These require manual creation. See Section 8.

### "Asset already exists, skipping" messages

This is expected on re-runs. The script is idempotent and skips existing assets to prevent data loss. To force recreation, delete the existing asset first.

### Pivot point is not at center-bottom

If meshes were created with the wrong pivot:
1. Open the Static Mesh Editor.
2. Use **Mesh** > **Pivot** > **Set to Center, Bottom** if available.
3. Or adjust the Pivot Offset in the Details panel:
   - X Offset = 0
   - Y Offset = 0
   - Z Offset = -(half the mesh height)

### Nanite not available

Nanite requires:
- A DirectX 12 capable GPU
- Windows 10/11
- Nanite-compatible project settings (enabled by default in UE 5.x)

If Nanite is unavailable, generate manual LODs instead:
1. In the Static Mesh Editor, go to **LOD Settings**.
2. Set **Number of LODs** to 3.
3. Use **Auto Generate** or set reduction percentages manually.

---

*This document is maintained alongside the Python script at*
*`D:\documents\Unreal Projects\game_maze\Content\Python\create_maze_assets.py`.*
*Both should be updated together when specifications change.*
