---
tags:
  - Player
  - Creature
  - Scene
---
# Goals
The goal of this design is to create a snappy, responsive controller for the Player to interact with the game.
# Components
- `Player:`[[Player (Class)]]
- `State:`[[StateLabel]]
- `Sprite2D:`[Sprite2D](https://docs.godotengine.org/en/stable/classes/class_sprite2d.html)
- `Hurtbox:`[CollisionShape2D](https://docs.godotengine.org/en/stable/classes/class_collisionshape2d.html)
- `Hitbox:`[Area2D](https://docs.godotengine.org/en/stable/classes/class_area2d.html)
	- `CollisionShape:`[CollisionShape2D](https://docs.godotengine.org/en/stable/classes/class_collisionshape2d.html)
- `InputBuffer:`[Timer](https://docs.godotengine.org/en/stable/classes/class_timer.html)
- `AnimationPlayer:`[AnimationPlayer](https://docs.godotengine.org/en/stable/classes/class_animationplayer.html)
- `StateMachine:`[[StateMachine]]
	- `Walk:`[[PlayerWalkState]]
	- `Fall:`[[PlayerFallState]]
	- `Jump:`[[PlayerJumpState]]
	- `Dodge:`[[PlayerDodgeState]]
	- `Parry:`[[PlayerParryState]]
	- `Block:`[[PlayerBlockState]]
	- `Light Attack:`[[PlayerLightAttackState]]
	- `Heavy Attack:`[[PlayerHeavyAttackState]]
# State Machine
The Player's [[StateMachine]] manages how the Player is controlled according to the following diagram:
![[Heathen Player State Machine.jpg]]
# Authors
- Code - Jollista
- Docs - Jollista