---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: [[Player (Scene)]] `!is_on_ground()`.
**Behavior**: handle [[Input]] for buffering.
# Inheritance
A PlayerFallState is-a [[State]].
# Variables
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `coyote_time:float` -> the amount of time after falling the player is allowed to [[Input]] jump to transition to [[PlayerJumpState]].
- `buffer_time:float` -> the amount of time before a buffered [[Input]] is reset.
- `air_max_speed:float` -> the maximum speed `player` can move in the air.
- `air_acceleration:float` -> the rate at which the `player`'s horizontal velocity changes in the air.
- `coyote:bool` -> used to indicate whether timer is currently being used for Coyote Time or Input Buffering.
- `exit_state:int` -> stores buffered [[Input]] within the range `enum{WALK, DODGE, PARRY, JUMP}`.
# Functions
## enter
Connect `player.input_buffer.timeout` to `_on_buffer_timeout` and start `player.input_buffer` with `coyote_time` if PlayerFallState was entered while `player.on_ground`.
## exit
Disconnect all connected signals, and reset `exit_state` to `WALK`.
## physics_update
Call `air_control()`, `handle_input()`, and `transition()` if `player.is_on_floor()`.
## handle_input
Buffer [[Input]].
## handle_jump
Transition to [[PlayerJumpState]] if in Coyote Time, else buffer [[Input]].
## transition
Transition to appropriate state ([[PlayerDodgeState]], [[PlayerParryState]], [[PlayerJumpState]], [[PlayerWalkState]]) based on `exit_state`.
## \_on\_buffer\_timeout
Forget buffered [[Input]].
## air\_control
Move while in the air.
# Authors
- Code - Jollista
- Docs - Jollista