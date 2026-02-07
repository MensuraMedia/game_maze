# Python Automation Templates for UE Editor

Ready-to-use Python scripts for common agent tasks. Execute in the UE Python console
or via `py "path/to/script.py"`.

> **Prerequisite:** Enable "Python Editor Script Plugin" in Edit > Plugins > Scripting

---

## Template 1: Spawn Maze Grid (Agent 03)

```python
import unreal
import random

def generate_maze_grid(rows=10, cols=10, cell_size=200.0, wall_height=300.0, seed=-1):
    """Generate a maze grid using recursive backtracking and spawn wall actors."""
    if seed >= 0:
        random.seed(seed)

    # Initialize grid: each cell has 4 walls (N, S, E, W)
    grid = [[{'N': True, 'S': True, 'E': True, 'W': True, 'visited': False}
             for _ in range(cols)] for _ in range(rows)]

    # Recursive backtracking
    def carve(r, c):
        grid[r][c]['visited'] = True
        directions = [('N', -1, 0, 'S'), ('S', 1, 0, 'N'),
                      ('E', 0, 1, 'W'), ('W', 0, -1, 'E')]
        random.shuffle(directions)
        for d, dr, dc, opposite in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and not grid[nr][nc]['visited']:
                grid[r][c][d] = False
                grid[nr][nc][opposite] = False
                carve(nr, nc)

    carve(0, 0)

    # Spawn walls in UE
    editor = unreal.EditorLevelLibrary
    wall_mesh_path = "/Game/Meshes/SM_Wall_01"
    floor_mesh_path = "/Game/Meshes/SM_Floor_01"

    for r in range(rows):
        for c in range(cols):
            base_x = c * cell_size
            base_y = r * cell_size

            # Spawn floor
            floor_loc = unreal.Vector(base_x, base_y, 0)
            # editor.spawn_actor_from_class(...)  # Configure per asset

            # Spawn walls where they exist
            cell = grid[r][c]
            if cell['N']:
                loc = unreal.Vector(base_x, base_y - cell_size / 2, wall_height / 2)
                # Spawn north wall actor
            if cell['E']:
                loc = unreal.Vector(base_x + cell_size / 2, base_y, wall_height / 2)
                # Spawn east wall actor (rotated 90 degrees)

    unreal.log(f"Maze generated: {rows}x{cols}, seed={seed}")
    return grid

# Execute
# generate_maze_grid(rows=10, cols=10, seed=42)
```

---

## Template 2: Asset Bulk Import (Agent 07)

```python
import unreal
import os

def bulk_import_meshes(source_dir, dest_path="/Game/Meshes"):
    """Import all FBX files from a directory into UE Content."""
    asset_tools = unreal.AssetToolsHelpers.get_asset_tools()
    tasks = []

    for filename in os.listdir(source_dir):
        if filename.lower().endswith('.fbx'):
            task = unreal.AssetImportTask()
            task.filename = os.path.join(source_dir, filename)
            task.destination_path = dest_path
            task.automated = True
            task.replace_existing = True
            task.save = True
            tasks.append(task)

    if tasks:
        asset_tools.import_asset_tasks(tasks)
        unreal.log(f"Imported {len(tasks)} meshes to {dest_path}")

# Execute
# bulk_import_meshes(r"D:\assets\maze_walls")
```

---

## Template 3: Place Lights Along Corridors (Agent 09)

```python
import unreal

def place_corridor_lights(spacing=3, cell_size=200.0, intensity=5000.0):
    """Place point lights at regular intervals along maze corridors."""
    editor = unreal.EditorLevelLibrary

    # Get all wall actors to determine corridor layout
    all_actors = editor.get_all_level_actors()
    floor_actors = [a for a in all_actors if "Floor" in a.get_name()]

    count = 0
    for i, actor in enumerate(floor_actors):
        if i % spacing == 0:
            loc = actor.get_actor_location()
            loc.z += 280.0  # Near ceiling
            light = editor.spawn_actor_from_class(
                unreal.PointLight, loc, unreal.Rotator(0, 0, 0)
            )
            light.point_light_component.set_intensity(intensity)
            light.point_light_component.set_light_color(
                unreal.LinearColor(1.0, 0.8, 0.5, 1.0)  # Warm torch color
            )
            count += 1

    unreal.log(f"Placed {count} corridor lights (spacing={spacing})")

# Execute
# place_corridor_lights(spacing=3)
```

---

## Template 4: Validate NavMesh Coverage (Agent 08)

```python
import unreal

def validate_navmesh(start_loc, end_loc):
    """Check if NavMesh path exists between two points."""
    nav_sys = unreal.NavigationSystemV1.get_navigation_system(
        unreal.EditorLevelLibrary.get_editor_world()
    )

    start = unreal.Vector(*start_loc)
    end = unreal.Vector(*end_loc)

    # Query path
    path = nav_sys.find_path_to_location_synchronously(
        unreal.EditorLevelLibrary.get_editor_world(), start, end
    )

    if path and path.is_valid():
        unreal.log(f"NavMesh path VALID: {len(path.path_points)} waypoints")
        return True
    else:
        unreal.log_warning("NavMesh path INVALID — maze may be unsolvable!")
        return False

# Execute after NavMesh rebuild
# validate_navmesh((0, 0, 50), (1800, 1800, 50))
```

---

## Template 5: Level Cleanup Utility (Agent 01 / Team Lead)

```python
import unreal

def cleanup_level(prefix="Maze_"):
    """Remove all actors matching a prefix (for maze regeneration)."""
    editor = unreal.EditorLevelLibrary
    all_actors = editor.get_all_level_actors()
    removed = 0

    for actor in all_actors:
        if actor.get_name().startswith(prefix):
            actor.destroy_actor()
            removed += 1

    unreal.log(f"Cleaned up {removed} actors with prefix '{prefix}'")

# Execute
# cleanup_level("Maze_")
```

---

## Template 6: Remote Control API Client (Any Agent)

```python
import requests
import json

UE_RC_URL = "http://localhost:30010/remote"

def call_ue_function(object_path, function_name, params=None):
    """Call a UE function via Remote Control API."""
    payload = {
        "objectPath": object_path,
        "functionName": function_name
    }
    if params:
        payload["parameters"] = params

    response = requests.post(f"{UE_RC_URL}/object/call", json=payload)
    return response.json()

def set_ue_property(object_path, property_name, value):
    """Set a UE object property via Remote Control API."""
    payload = {
        "objectPath": object_path,
        "propertyName": property_name,
        "propertyValue": value
    }
    response = requests.post(f"{UE_RC_URL}/object/property", json=payload)
    return response.json()

# Example: Trigger maze regeneration remotely
# call_ue_function(
#     "/Game/Maps/MazeLevel.MazeLevel:PersistentLevel.BP_MazeGenerator_1",
#     "GenerateMaze",
#     {"Rows": 15, "Cols": 15}
# )
```

---

*Add new templates as agents discover or create new automation patterns.*
