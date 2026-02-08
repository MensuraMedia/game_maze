# Sprint Board — Maze Game

## Sprint 1: Foundation (Core Systems)

| ID | Task | Agent | Status | Blocks | Blocked By |
|----|-------|-------|--------|--------|------------|
| T-001 | Write Game Design Document | 02 Game Director | **Done** | T-002–T-010 | — |
| T-002 | Validate UE5 project setup | 01 Team Lead | **Done** | T-003–T-010 | T-001 |
| T-003 | Implement procedural maze generator (C++) | 03 Level Design + 06 C++ | **Done** | T-005, T-006, T-008 | T-001, T-002 |
| T-004 | Set up player character controller | 04 Character Controller | **Done** | T-006, T-009 | T-001, T-002 |
| T-005 | Create modular wall/floor/ceiling assets | 07 Environment | **Done** | T-003 (runtime), T-007 | T-001 |

## Sprint 2: Gameplay Loop

| ID | Task | Agent | Status | Blocks | Blocked By |
|----|-------|-------|--------|--------|------------|
| T-006 | Implement collectible system (Blueprints) | 05 Blueprints | **Done** | T-010 | T-003, T-004 |
| T-007 | Set up lighting & atmosphere | 09 Lighting | **Done** | T-009 | T-003, T-005 |
| T-008 | Configure NavMesh & validate solvability | 08 AI/Nav | **Done** | T-006 | T-003 |
| T-009 | Implement footstep audio system | 10 Audio | **Done** | — | T-004, T-005 |
| T-010 | Implement win/lose conditions & game mode | 06 C++ | **Done** | T-011 | T-003, T-006 |

## Sprint 3: UI & Polish

| ID | Task | Agent | Status | Blocks | Blocked By |
|----|-------|-------|--------|--------|------------|
| T-011 | Build HUD (timer, score, crosshair) | 11 UI/UX | **Done** | T-012 | T-010 |
| T-012 | Implement minimap system | 11 UI/UX | **In Progress** | — | T-003, T-011 |
| T-013 | Create main menu & pause menu | 11 UI/UX | **Done** | — | T-010 |
| T-014 | Add ambient audio & sound effects | 10 Audio | **Done** | — | T-007 |
| T-015 | Post-processing & visual polish | 09 Lighting | **Done** | — | T-007 |

## Sprint 4: Integration & QA

| ID | Task | Agent | Status | Blocks | Blocked By |
|----|-------|-------|--------|--------|------------|
| T-016 | Full integration build & playtest | 00 Conductor | Pending | T-017 | T-011–T-015 |
| T-017 | Bug fixing & performance optimization | 01 Team Lead + 06 C++ | Pending | T-018 | T-016 |
| T-018 | Final polish pass & release build | 00 Conductor | Pending | — | T-017 |

---
*Maintained by Agent 01 (Team Lead). Updated by Conductor after each cycle.*
