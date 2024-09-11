---
tags:
  - Player
  - Script
---
# Goals
The goal of the Player Class is to provide a base for the [[Player (Scene)]]'s components to use, such as the 
# Inheritance
A Player is-a [[Creature]].
# Variables
- `gravity` -> the gravity applied to the player while `!is_on_floor()`
- `on_ground:bool` -> used by [[PlayerWalkState]], [[PlayerFallState]], and [[PlayerJumpState]] for [Coyote Time](https://www.youtube.com/watch?v=ePoNO5P1Csc).
- `animation:`[AnimationPlayer](https://docs.godotengine.org/en/stable/classes/class_animationplayer.html) -> a reference to the [[Player (Scene)]]'s component `AnimationPlayer`.
- `sprite:`[Sprite2D](https://docs.godotengine.org/en/stable/classes/class_sprite2d.html) -> a reference to the [[Player (Scene)]]'s component `Sprite2D`.
- `input_buffer:`[Timer](https://docs.godotengine.org/en/stable/classes/class_timer.html) -> a reference to the [[Player (Scene)]]'s component `InputBuffer`.
- `hitbox:`[Area2D](https://docs.godotengine.org/en/stable/classes/class_area2d.html) -> a reference to the [[Player (Scene)]]'s component `Hitbox`.
# Functions
## \_physics\_process
Applies gravity to vertical velocity if 
### Parameters
- `delta` -> seconds since the previous frame.
# Authors
- Code - Jollista
- Docs - Jollista