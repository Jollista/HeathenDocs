---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: [[Input]] jump.
**Behavior**: jump while allowing [[Input]] walk.
# Inheritance
A PlayerJumpState is-a [[State]].
# Variables
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `jump_force:float` -> the force applied to `player` on `enter()`.
- `air_max_speed:float` -> the maximum speed `player` can move in the air.
- `air_acceleration:float` -> the rate at which the `player`'s horizontal velocity changes in the air.
# Functions
## enter
Set `player.on_ground` to false, and apply `jump_force` to `player.velocity.y` to make it jump.
## update
Once `player` reaches the apex of its jump, transition to [[PlayerFallState]].
## physics_update
Allow [[Input]] walk to affect `player.velocity.x`.
# Authors
- Code - Jollista
- Docs - Jollista