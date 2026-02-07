# Skill: Win/Lose Conditions

## ID
`win-lose`

## Description
Implement game completion logic: win on reaching exit, optional lose on timer expiry,
with appropriate screens and replay flow.

## Agents
Primary: Agent 06 (C++ Programmer) | Support: Agent 11 (UI/UX), Agent 10 (Audio)

## Inputs
| Parameter | Type | Default | Description |
|---|---|---|---|
| WinCondition | enum | ReachExit | Primary win trigger |
| TimerEnabled | bool | true | Enable countdown timer |
| TimerSeconds | float | 120.0 | Time limit |
| ShowStats | bool | true | Show completion stats on win |

## Steps
1. In MazeGameMode: define game states (Playing, Won, Lost)
2. Place exit trigger volume at maze exit cell
3. On player overlap with exit → transition to Won state
4. If timer enabled: count down in GameState, broadcast on expiry → Lost state
5. On Won: show WBP_WinScreen (time, score, collectibles found)
6. On Lost: show WBP_LoseScreen (progress percentage)
7. Both screens offer: Retry (regenerate maze), Quit to Menu
8. Play appropriate audio sting on state change

## Output
- Complete game loop: Play → Win/Lose → Retry/Quit
- Stats tracking (time, score, completion %)
- Clean state reset on retry
