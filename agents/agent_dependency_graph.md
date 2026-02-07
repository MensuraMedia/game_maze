# Agent Dependency Graph

```
                    ┌──────────────────┐
                    │  00 CONDUCTOR    │
                    │  (Orchestrator)  │
                    └────────┬─────────┘
                             │
                    ┌────────┴─────────┐
                    │  01 TEAM LEAD    │
                    │  (Task Manager)  │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
     ┌────────┴───┐  ┌──────┴─────┐  ┌────┴────────┐
     │ 02 GAME    │  │ 06 C++     │  │ 05 BLUEPRINT│
     │ DIRECTOR   │  │ PROGRAMMER │  │ AGENT       │
     └──────┬─────┘  └──────┬─────┘  └──────┬──────┘
            │               │               │
    ┌───────┴───────────────┤               │
    │       │               │               │
┌───┴───┐ ┌─┴──────┐ ┌─────┴─────┐  ┌──────┴──────┐
│03 LVL │ │04 CHAR │ │07 ENV     │  │08 AI/NAV   │
│DESIGN │ │CONTROL │ │ARTIST     │  │            │
└───┬───┘ └───┬────┘ └─────┬─────┘  └────────────┘
    │         │             │
    │    ┌────┴────┐   ┌────┴────┐
    │    │10 AUDIO │   │09 LIGHT │
    │    │& SFX    │   │& ATMOS  │
    │    └─────────┘   └─────────┘
    │
    └──────────┐
         ┌─────┴─────┐
         │11 UI/UX   │
         │& POLISH   │
         └───────────┘
```

## Execution Order (Critical Path)

1. **Phase 0:** 02 Game Director → GDD (unblocks everything)
2. **Phase 1 (parallel):**
   - 03 Level Designer → Maze generation
   - 04 Character Controller → FPS movement
   - 07 Environment Artist → Modular assets
3. **Phase 2 (parallel, after Phase 1):**
   - 06 C++ Programmer → GameMode, win/lose, optimization
   - 05 Blueprint Agent → Collectibles, events, interactions
   - 08 AI/Navigation → NavMesh, validation
4. **Phase 3 (parallel, after Phase 2):**
   - 09 Lighting & Atmosphere → Lights, fog, post-process
   - 10 Audio & SFX → Footsteps, ambient, stings
5. **Phase 4:** 11 UI/UX → HUD, minimap, menus
6. **Phase 5:** 00 Conductor + 01 Team Lead → Integration, QA, polish
