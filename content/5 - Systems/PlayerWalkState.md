---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: Horizontal movement buttons held down, or nothing is pressed.
**Behavior**: Horizontal movement and idle.
# Inheritance
A PlayerWalkState is-a [[State]].
# Variables
- `walk_speed:float` -> the speed the `player` moves when input is detected.
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `friction:float` -> //deprecated, can be safely removed
- `direction:float` -> //doesn't need to be globally scoped, can be changed to a local variable in `walk()`
# Functions
## enter
Sets `player.on_ground` to true.
## update
Updates `player.sprite.flip_h` based on `player.velocity.x`.
## physics_update
Calls `walk()` and `transition()`.
## walk
Update player's horizontal velocity based on [[Input]] walk_left/walk_right.
## transition
- if `!player.is_on_floor()` -> transition to [[PlayerFallState]]
- if [[Input]] dodge -> transition to [[PlayerDodgeState]]
- if [[Input]] parry -> transition to [[PlayerParryState]]
- if [[Input]] jump -> transition to [[PlayerJumpState]]
- if [[Input]] attack -> transition to [[PlayerLightAttackState]]
# Authors
- Code - Jollista
- Docs - Jollista