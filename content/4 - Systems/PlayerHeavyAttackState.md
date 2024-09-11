---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: [[Input]] attack held.
**Behavior**: charge up a heavy attack until attack deals maximum damage or player releases attack [[Input]].
# Inheritance
A PlayerHeavyAttackState is-a [[State]].
# Variables
- `exit_state:int` -> stores buffered [[Input]] within the range `enum{DODGE, PARRY, JUMP, WALK}`.
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `buffer_time:float` -> the amount of time before a buffered [[Input]] is reset.
- `max_health_damage:int` -> [[Health and Structure|Health]] damage dealt on full charge.
- `min_health_damage:int` -> [[Health and Structure|Health]] damage dealt on minimum charge.
- `max_structure_damage:int` -> [[Health and Structure|Structure]] damage dealt on full charge.
- `min_structure_damage:int` -> [[Health and Structure|Structure]] damage dealt on minimum charge.
- `velocity:`[Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) -> velocity to be applied to `player`, animated by attack animations.
- `hitbox_scale:`[Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) -> scale to be applied to `player.hitbox`, animated by attack animations.
- `hitbox_position:`[Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) -> position to be applied to `player.hitbox`, animated by attack animations.
- `direction:int` -> stores the direction of the attack (-1 left, 1 right).
- `alpha:float` -> stores the alpha between minimum and maximum damage variables, determined by the amount of time the attack is charged range: \[0, 1\].
# Functions
## enter
Initialize `exit_state` and `direction`, connect signals for `_on_animation_finished()`, `_on_buffer_timeout()`, and `_on_body_entered_hitbox` and play `player.animation`'s animation for heavy attack startup.
## exit
Disconnect all connected signals.
## physics_update
`handle_input()` and `apply_attack_modifiers()`.
## handle_input
If attack is just starting up, allow [[Input]] to cancel into [[PlayerDodgeState]], [[PlayerParryState]], or [[PlayerJumpState]], else if [[Input]] attack is released, play the heavy\_attack animation. If the heavy\_attack animation is playing, `buffer_defensive_states()`.
## buffer_defensive_states
Buffer [[Input]] for dodge, parry, and jump.
## buffer_input
Buffer [[Input]] to `exit_state`.
### Parameters
- `input` -> the `exit_state` associated with the [[Input]] buffered (expected within range `enum{DODGE, PARRY, JUMP, WALK}`).
## apply\_attack\_modifiers
Apply `velocity` to `player.velocity`, and `hitbox_scale` and `hitbox_position` to `player.hitbox` with respect to `direction`.
## \_on\_animation\_finished
If finished animation was heavy\_attack\_startup, play heavy\_attack. If animation was heavy\_attack, `transition()`.
### Parameters
- `anim_name:StringName` -> name of the animation just finished.
## transition
Transition to appropriate state ([[PlayerWalkState]], [[PlayerDodgeState]], [[PlayerParryState]], [[PlayerJumpState]]) based on `exit_state`.
## \_on\_buffer\_timeout
Reset `exit_state` to `WALK`.
## \_on\_body\_entered\_hitbox
If `body` can `take_damage()`, take [[Health and Structure|Health]] damage equal to the `alpha` along a linear interpolation between `min_health_damage` and `max_health_damage`; [[Health and Structure|Structure]] damage equal to the `alpha` between `min_structure_damage` and `max_structure_damage`.
### Parameters
- `body:`[Node2D](https://docs.godotengine.org/en/stable/classes/class_node2d.html) -> reference to the body that just entered `player.hitbox`.
# Authors
- Code - Jollista
- Docs - Jollista