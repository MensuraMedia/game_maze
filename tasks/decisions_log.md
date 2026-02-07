# Technical Decisions Log

Maintained by Agent 01 (Team Lead). Records all binding technical decisions.

---

| Date | Decision | Rationale | Decided By |
|------|----------|-----------|------------|
| — | Use UE5 First Person template as base | Provides camera, input, and character scaffolding out of the box | Team Lead |
| — | Maze gen in C++, gameplay logic in Blueprints | C++ for performance-critical generation; Blueprints for rapid iteration on gameplay | Team Lead |
| — | Recursive backtracking as default algorithm | Simple, guaranteed to produce perfect mazes, easy to extend | Level Design + C++ |
| — | 200x200x300 UU cell size | Fits comfortably with default character capsule, good corridor feel | Team Lead + Environment |
| — | Enhanced Input System (not legacy) | UE5 standard, supports rebinding, better gamepad support | Character Controller |
| — | Instanced Static Meshes for walls | Orders of magnitude better draw call performance vs individual actors | C++ Programmer |
| Sprint 1 | UE 5.7 with Lumen + HW RT + Nanite | Project already configured for high-end rendering; no baked lighting needed | Team Lead |
| Sprint 1 | C++ module name: game_maze | Matches .uproject name, avoids rename friction | Team Lead |
| Sprint 1 | Enable PythonScriptPlugin, RemoteControl, PCG plugins | Required by 002_maze_guide agent interface directives | Team Lead |
| Sprint 1 | Keep existing FP Blueprint assets as reference | Extend via C++ rather than delete; BP versions serve as prototyping baseline | Team Lead |

---
*Add new decisions as rows. Never delete — strike through if reversed.*
