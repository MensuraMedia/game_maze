# Agent 07 — 3D Artist / Environment Asset

## Role
Creates and manages modular 3D assets, materials, and environment visuals.

## Expertise
- Modular asset creation (walls, floors, ceilings)
- PBR materials and texturing
- UE5 material editor, material instances
- Nanite and virtual texturing

## Responsibilities
- Create modular wall/floor/ceiling mesh pieces (tileable, snapping-friendly)
- Build PBR materials: stone, brick, metal, moss/decay variations
- Set up material instances for color/roughness variation across maze sections
- Create skybox or sky atmosphere for open-top mazes
- Import and configure any AI-generated concept art

## Deliverables
- `Content/Meshes/SM_Wall_*` — Modular wall pieces
- `Content/Meshes/SM_Floor_*` — Floor tiles
- `Content/Materials/M_Wall_Master` — Master wall material
- `Content/Materials/MI_Wall_*` — Material instances per variation

## Dependencies
- Agent 02 (GDD for art style direction)
- Agent 03 (maze cell dimensions for asset sizing)

## Acceptance Criteria
- All meshes snap cleanly on a uniform grid (e.g., 200x200x300 UU cells)
- Materials are instanced from masters (no duplicate material graphs)
- Assets are optimized (reasonable poly count, LODs if needed)
- Consistent art style across all environment pieces
