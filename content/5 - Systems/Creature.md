---
tags:
  - Creature
  - Script
---
# Goals
The goal of the Creature script is to provide a class which Nodes can use to inherit a standard means of taking [[Health and Structure]] Damage.
# Inheritance
A Creature is-a [CharacterBody2D](https://docs.godotengine.org/en/latest/classes/class_characterbody2d.html).
# Variables
- `max_health:int` -> the maximum [[Health and Structure|Health]] the Creature can have at any given time.
- `current_health:int` -> the current amount of [[Health and Structure|Health]] the Creature has. At zero or less health, the Creature is [[Health and Structure|Dead]].
- `max_structure:int` -> the maximum [[Health and Structure|Structure]] the Creature can have at any given time.
- `current_structure:int` -> the current amount of [[Health and Structure|Structure]] the Creature has. At zero or less Structure, the creature is [[Health and Structure|Staggered]].
- `state_machine:`[[StateMachine]] -> a reference to the [[StateMachine]] at $StateMachine [onready](https://docs.godotengine.org/en/latest/tutorials/scripting/c_sharp/c_sharp_differences.html#onready-annotation) used for transitions between states.
# Signals
- `parry_successful` -> emitted by [[Creature#parry|parry]] function
# Functions
## take_damage
Take damage to health and structure from origin. [[Creature#parry|Parry]] if parrying, ignore damage if [[Health and Structure|Invincible]], [[Health and Structure|Die or Stagger]] if Health or Structure are reduced to zero or less.
### Parameters
- `origin` -> a reference to the source of the damage
- `health_damage:int` -> the amount of [[Health and Structure|Health]] damage to take if the attack is successful
- `structure_damage:int` -> the amount of [[Health and Structure|Structure]] damage to take if the attack is successful
## heal
Increment `current_health` and `current_structure` according to parameters, within bounds of `max_health` and `max_structure`.
## Parameters
- `health:int` -> the amount of [[Health and Structure|Health]] to increment by.
- `structure:int` -> the amount of [[Health and Structure|Structure]] to increment by.
## parry
Parry `target` if it has-a method `get_parried`. Parry deals structure damage to `target` according to the following formula:
> `target_structure_damage` \* 2
## Parameters
- `target` -> the target to be parried
- `target_structure_damage:int` -> the amount of structure damage the target would have dealt to the parrying Creature.
## get_parried
Take [[Health and Structure|Structure]] damage according to parameters.
### Parameters
- `origin` -> the origin of the [[Health and Structure|Structure]] damage.
- `structure_damage:int` -> the amount of [[Health and Structure|Structure]] damage to take.
# Authors
- Code - Jollista
- Docs - Jollista