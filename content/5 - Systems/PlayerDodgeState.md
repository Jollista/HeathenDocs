---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: [[Input]] dodge.
**Behavior**: move horizontally while temporarily invulnerable.
# Inheritance
A PlayerDodgeState is-a [[State]].
# Variables
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `dodge_speed_peak:float` -> highest speed in dodge animation.
- `dodge_speed_low:float` -> lowest speed in dodge animation.
- `dodge_speed:float` -> actual speed of the dodge.
- `alpha:float` -> weight between speeds, animated by [[Player (Scene)]]'s `AnimationPlayer`.
- `buffer_time:float` -> the amount of time before a buffered [[Input]] is reset.
- `direction:int` -> direction for the player to dodge in (-1 left, 1 right).
- `exit_state:int` -> stores buffered [[Input]] within the range `enum{WALK, PARRY, JUMP, LIGHT_ATTACK}`.
# Functions
## enter
Connect `player.input_buffer.timeout` to `_on_buffer_timeout()` and `player.animation.animation_finished` to `_on_animation_finished()`, then initialize `direction`, default `dodge_speed` to `dodge_speed_peak`. Finally play `player.animation`'s dodge animation.
## exit
Disconnect all connected signals and reset `exit_state` to `WALK`.
## physics_update
`handle_input()` and `update_velocity()`.
## handle_input
Buffer [[Input]] for [[PlayerParryState]], [[PlayerJumpState]], and [[PlayerLightAttackState]].
## update_velocity
Update `player.velocity.x` by the `alpha` between `dodge_speed_peak`, and `dodge_speed_low`.
## get_direction
Determine `direction` based on walk [[Input]].
## \_on\_buffer\_timeout
Clear buffered [[Input]] except parry if parry input is still detected.
## \_on\_animation\_finished
If finished animation was dodge, `transition()`.
## transition
- if `!player.is_on_floor()` -> transition to [[PlayerFallState]]
- else, transition to [[PlayerWalkState]], [[PlayerParryState]], [[PlayerJumpState]], or [[PlayerLightAttackState]] based on `exit_state`
# Authors
- Code - Jollista
- Docs - Jollista