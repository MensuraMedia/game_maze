# Skill: Procedural Maze Generation

## ID
`maze-gen`

## Description
Generate a solvable 3D maze using procedural algorithms, output as a grid of cells
with wall data, and spawn corresponding UE5 actors.

## Agents
Primary: Agent 03 (Level Designer) | Support: Agent 06 (C++ Programmer)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| Rows | int | 10 | Number of maze rows |
| Cols | int | 10 | Number of maze columns |
| CellSize | float | 200.0 | World-unit size per cell |
| WallHeight | float | 300.0 | Wall height in world units |
| Seed | int | -1 | Random seed (-1 = random) |
| Algorithm | enum | RecursiveBacktrack | Generation algorithm |

## Steps
1. Allocate 2D grid of cells, all walls initially present
2. Run selected algorithm (recursive backtracking / Prim's / Kruskal's)
3. Place start cell (0,0) and exit cell (Rows-1, Cols-1)
4. Validate solvability via flood-fill from start to exit
5. For each cell, spawn wall actors for remaining walls using Instanced Static Meshes
6. Spawn floor plane and optional ceiling
7. Place PlayerStart at start cell, exit trigger at exit cell

## Output
- Populated maze in UE5 world
- Maze data struct (serializable for save/load)
- Solvability confirmation

## Performance Target
< 1 second for 50x50 maze on mid-range hardware
