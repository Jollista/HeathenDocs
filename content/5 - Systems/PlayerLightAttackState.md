---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: [[Input]] attack one or more times in a sequence.
**Behavior**: attack quickly once or multiple times.
# Inheritance
A PlayerLightAttackState is-a [[State]].
# Variables
- `exit_state:int` -> stores buffered [[Input]] within the range `enum{LIGHT_ATTACK, HEAVY_ATTACK, DODGE, PARRY, JUMP, WALK}`.
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `attack_animations:Array` -> an array of names for attack animations.
- `attack:int` -> index of the current attack in `attack_animations`.
- `buffer_time:float` -> the amount of time before a buffered [[Input]] is reset.
- `velocity:`[Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) -> velocity to be applied to `player`, animated by attack animations.
- `hitbox_scale:`[Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) -> scale to be applied to `player.hitbox`, animated by attack animations.
- `hitbox_position:`[Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) -> position to be applied to `player.hitbox`, animated by attack animations.
- `direction:int` -> stores the direction of the attack (-1 left, 1 right).
- `health_damage:int` -> amount of [[Health and Structure|Health]] damage dealt by each attack, animated by each attack animation in `attack_animations`.
- `structure_damage:int` -> amount of [[Health and Structure|Structure]] damage dealt by each attack, animated by each attack animation in `attack_animations`.
# Functions
## enter
Initialize variables, default `exit_state` to `HEAVY_ATTACK`. Connect signals for `_on_animation_finished()`, `_on_buffer_timeout()`, and `_on_body_entered_hitbox()`. Then, play attack\_startup animation.
## exit
Disconnect all connected signals.
## physics_update
`handle_input()` and `apply_attack_modifiers()`.
## apply\_attack\_modifiers
Apply `velocity` to `player.velocity`, and `hitbox_scale` and `hitbox_position` to `player.hitbox` with respect to `direction`.
## get_direction
Determine `direction` based on walk [[Input]].
## handle_input
If in attack\_startup, and not [[Input]] attack, set `exit_state` to `LIGHT_ATTACK` and `buffer_defensive_states()`. Else, `buffer_defensive_states()` and buffer [[Input]] attack.
## buffer_defensive_states
Buffer [[Input]] for dodge, parry, and jump.
## buffer_input
Buffer [[Input]] to `exit_state`.
## \_on\_animation\_finished
If attack\_startup animation finished and `exit_state` is `HEAVY_ATTACK` transition to [[PlayerHeavyAttackState]]. Otherwise, `transition()`.
### Parameters
- `anim_name:StringName` -> the name of the animation just finished.
## transition
Start `next_attack()` or transition to appropriate state ([[PlayerWalkState]], [[PlayerDodgeState]], [[PlayerParryState]], [[PlayerJumpState]]) based on `exit_state`.
## next\_attack
Reset `exit_state` to `WALK` and play next attack animation in `attack_animations` if there's more.
## end\_sequence
Transition to [[PlayerWalkState]] after finishing a sequence of attacks.
## \_on\_buffer\_timeout
If finished attack\_startup animation, set `exit_state` to `HEAVY_ATTACK`. Else, `WALK`.
## \_on\_body\_entered\_hitbox
If `body` can `take_damage()`, take [[Health and Structure]] damage according to `health_damage` and `structure_damage`.
### Parameters
- `body:`[Node2D](https://docs.godotengine.org/en/stable/classes/class_node2d.html) -> reference to the body that just entered `player.hitbox`.
# Authors
- Code - Jollista
- Docs - Jollista