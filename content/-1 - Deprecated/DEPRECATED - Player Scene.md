---
tags:
  - Player
  - DEPRECATED
---
//This is a conversion of some old, disorganized documentation. The full original document can be found [here](https://docs.google.com/document/d/1-xWPC0GV3b-EhCuFb-PIVHam5nWAMFICwly4Hubqiq8/edit?usp=sharing) with comments included.

This document is so I and hopefully everyone else also will be able to understand what it is we’re doing in the code. Basically, this is to explain design and implementation stuff. Most of the detailed function explanations are written in natural language for ease of understanding.
  
As a disclaimer, I’m not sure if this design will work 100% well. I think it’ll work decently well enough for what I think that we want to do, but I’m not entirely sure of the intricacies of what we want to do, so I made a number of assumptions. Everyone with the link has comment permissions, so please feel free to comment feedback directly on this document or in the discord. Anything and everything is subject to change, and if people like this design and we decide to go with it or a version of it, I’ll give everyone edit permissions for this doc so we can keep some organized documentation for everything else, too.
# The Player

An explanation of everything going on with the player.
## Player Scene
### Player
The root node of the scene is a Creature which is a CharacterBody2D. Attached is the script that manages health, max_health, structure, max_structure, staggered, on_ground, inventory(??), and a number of functions
#### deal_damage(damage:int, structure_damage:int):
For each object in hitbox, if it has a function take_damage(damage:int, structure_damage:int) and isn’t self, it calls it passing in the argument damage for damage and structure_damage for structure_damage
#### \_physics\_process(\_delta):
Constantly applies gravity and calls move_and_slide()
### AnimationPlayer
Used to play animations which animate the character’s Sprite2D, different variables to keep track of states and modify how the player gives and takes damage, hitboxes, and whatever else it needs to. Likely sound effects as well. 

AnimationPlayers in godot are very powerful tools because they can do a lot, even calling functions and stuff, so it’s used a lot in this design, especially for attacks and the timing of dodges, parrying, and blocking.
### Hitbox
An Area2D used by attack animations to determine the area in which things take damage when attacking
### Input Buffer
A timer that is used for a few input buffering techniques. Namely, it’s used to measure out coyote time and other similar forms of timed input buffering. 

Input buffering is whenever the player makes an input when it’s not normally valid and we hold onto it to use later to provide them with some grace that creates the illusion of responsiveness. Coyote time is a form of input buffering where the player is allowed to jump within a certain window of time after falling off a ledge. 

If this doesn’t make sense, here’s a [better explanation](https://youtu.be/ePoNO5P1Csc?si=tyWtrPXuQA6aZZaV) of coyote time and input buffering.
#### Coyote Time
This implementation is relatively simple. The player has a timer node, Input Buffer. When the player is no longer on the ground, the falling state is entered and the timer is set to coyote_time and started.

If the jump input is received, and the timer is still running, the player jumps. Else, if the timer is stopped, it’s set to reverse_coyote_time and started, and jumping is set to true. Then, if the player lands and the timer is still running and jumping is true, the player transitions to jump instead of walk.
## Heathen Player State Machine
The player is controlled in part with a finite state machine. The reason for this is that there’s a lotta stuff this guy can do, and I figured it’d be a whole lot easier to understand piece by piece rather than all being in one script. Principle of single responsibility and all that.

For this project, a state is a set of conditions and a related behavior. While the condition is met, the behavior is active. For example, a state “blocking” would have the condition of the player holding down the block button and the behavior of blocking attacks. Each of these states has a function enter() which defines the behavior when entering the state, exit() which defines the behavior when exiting the state, update() which is called each frame while active (called continuously in state machine’s _process() function), and physics_update() which is called each physics tick while active (called continuously in state machine’s _physics_process() function).

A state machine is a collection of states. It’s responsible for managing transitions between states (calling the exit() function of the previous state and the enter() function of the new one) as well as calling the update() and physics_update() functions of the active state. The cool thing here is that it doesn’t need to know any information about the specific states, so we can add or subtract as many states as we want and the same state machine will work.

If this doesn’t make sense, here’s a [better explanation](https://youtu.be/ow_Lum-Agbs?si=KP1sVdBiC1VwIISA) on state machines in Godot. This design should function similarly to the state machine example in that video.
### Image Reference
![[Heathen Player State Machine.jpg]]
## States
### Walk
**Condition**: Horizontal movement buttons held down, or nothing is pressed.
**Behavior**: Horizontal movement and idle.
#### enter():
Set **on_ground** to true
#### exit():
pass
#### update():
Update animation:
If the magnitude of **velocity** is zero, play idle animation and return.
Else, flip the sprite to match the direction of the velocity and move in that direction

If player presses dodge button, transition to Dodge
If player presses block button, transition to Parry
If player presses jump button, transition to Jump
If player presses attack button, transition to Light Attack
If player is not on floor, transition to Fall
#### physics_update():
Add input to velocity
#### Transitions to
- Jump on jump button pressed
- Dodge on dodge button pressed
- Parry on block button pressed
- Light Attack on attack button pressed
- Fall when no longer on ground
### Fall
Condition: Player is not on ground
Behavior: Fall
#### enter():
If on_ground set the timer InputBuffer to coyote_time and start it. on_ground = false
Play falling animation
#### exit():
exit_state = WALK
#### update():
pass
#### physics_update():
// handle attack input
If attack input received, transition to Plunging Attack
// handle dodge input
If dodge input received, set InputBuffer to buffer_time and set exit_state to DODGE
// handle parry input
If block input received, set InputBuffer to buffer_time and set exit_state to PARRY
// handle jump input
If jump input received
If coyote and InputBuffer is not stopped, we’re in coyote time -> Transition to jump
Else, set InputBuffer to buffer_time and set exit_state to JUMP
// handle transitions
If on floor and exit_state is DODGE transition to Dodge
Else, if on floor and exit_state is PARRY transition to Parry
Else, if on floor and exit_state is JUMP transition to Jump
Else, transition to Walk
#### \_on\_input\_timeout():
exit_state = WALK
#### Transitions to
- Walk on landed
- Plunging Attack on attack button pressed
- Dodge on landed and exit_state is DODGE
- Parry on landed and exit_state is PARRY
- Jump on landed and exit_state is JUMP
### Jump
Condition: Jump button pressed.
Behavior: Jump
#### enter():
Play jump animation and jump (apply jump force to velocity and set on_ground to false)
#### exit():
pass
#### update():
If velocity.y >= 0 (down)
Transition to falling
#### physics_update():
pass
#### Transitions to
- Fall when falling
### Dodge
Condition: Dodge button pressed
Behavior: A dodge roll to avoid attacks and move horizontally.
#### enter():
Play dodge animation
Use animation player to set player to be invincible, and disable invincible after a certain amount of time
Use animation player to move dodge_length in direction at a factor of dodge_speed
#### exit():
pass
#### update():
If input is received for block, jump, or attack, set the exit_state to its respective enum (PARRY, JUMP, ATTACK) to true and the others to false and set InputBuffer to buffer_time and start.

Parry should be independent of the timer (since it must be held) and use Input.is_action_pressed, the rest should use Input.is_action_just_pressed.

The priority of inputs should be as follows: Parry > Jump > Attack
#### physics_update():
pass
#### \_on\_input\_timeout():
exit_state = WALK
#### \_on\_animation\_ended():
When dodge animation ends, transition to the next state depending on exit_state
#### Transitions to
- Parry on input buffered parry
- Jump on input buffered jump
- Light Attack on input buffered attack
- Walk on no input received
### Parry
Condition: First moments after block button pressed
Behavior: A timed block to prevent damage
#### enter():
Play parry animation (Player pulls sword into blocking position)
Animation player should set player to be invincible
#### exit():
pass
#### update():
If player is attacked while animation player is playing parry animation, end parry animation and play successful_parry animation which should damage the parried enemy’s structure by a lot
#### physics_update():
pass
#### \_on\_animation\_ended():
When successful_parry or parry animations end, transition to Block state.
#### Transitions to
- Block on parry or successful_parry animations ended
### Block
Condition: Block button held
Behavior: A block to prevent damage at the cost of structure.
#### enter():
Play block animation on a loop
#### exit():
Stop block animation
#### update():
pass
#### physics_update():
If dodge input received, transition to Dodge
Else, if block button is no longer held down, play block_end animation (this adds ending lag to blocking, so player can’t just spam the block button for infinite parry)
#### \_on\_animation\_ended():
When block_end animation ends, transition to walk.
#### Transitions to
- Walk when block button is no longer held
- Dodge when dodge button is pressed
### Light Attack
Condition: Attack button pressed
Behavior: A light attack, sequence of light attacks, or the starting lag of a heavy attack.
#### enter():
Set charging to true
#### exit():
Reset current_attack to 0
#### update():
If attack button is ever released, set charging to false
If current animation is an attack animation, and the player inputs the attack button, set attacking to true
If animation player is playing an animation, and the player inputs the dodge button, set dodging to true
If animation player is playing an animation, and the player inputs the block button, set parrying to true
#### physics_update():
pass
#### \_on\_animation\_ended():
When attack_startup animation ends, 
If dodging, transition to Dodge
If parrying, transition to Parry
If charging, transition to Heavy Attack. 
Else, play the animation attack_animations[current_attack] (which animates hurtbox and calls deal_damage(damage, structure_damage)) and increment current_attack by 1

When an attack animation ends, 
If dodging, transition to Dodge
If parrying, transition to Parry
If attacking is true, play attack_startup animation
#### Transitions to
- Heavy Attack when attack_startup animation ends and charging
- Walk when attack animation ends end and (not attacking or current_attack >= length of attack_animations)
- Dodge when animation ends and dodging
- Parry when animation ends and parrying but not dodging
### Heavy Attack
Condition: Attack button held (oh my god just like sekiro)
Behavior: A charged heavy attack
#### enter():
Play heavy_charge animation
#### exit():
pass
#### update():
If dodge button is pressed, 
If playing heavy_charge animation, stop and transition to Dodge
If playing heavy_attack animation, set dodging to true

When heavy_charge animation is playing and attack button is released, set current_damage to lerp(min_damage, max_damage, amount of time heavy_charge played for divided by the animation’s length). This sets current_damage to a point between min and max damage.

Play heavy_attack animation, which animates hurtbox and calls deal_damage()
#### physics_update():
pass
#### \_on\_animation\_ended():
When heavy_charge animation ends, set current_damage to max_damage and play heavy_attack animation

When heavy_attack animation ends, 
If dodging, transition to Dodge
Else, transition to Walk
#### Transitions to
- Walk after attack animation ends
- Dodge when dodge is pressed while charging
### Wallrun
Condition: 
Behavior: 
### Plunging Attack
Condition: Attack button pressed while in the air or wallrunning
Behavior: A downward slash
#### enter():
Play plunging attack animation
#### exit():
on_ground = true
#### update():
If player is_on_floor(), play plunging attack finish animation, which should animate the hurtbox and call deal damage
#### physics_update():
Apply gravity
move_and_slide()
#### \_on\_animation\_ended():
When plunging attack finish animation ends, transition to Walk
#### Transitions to
- Walk after attack
### Staggered
Condition: Structure <= 0
Behavior: Staggered temporarily and open to critical damage
#### enter():
Play stagger animation, which should set player to staggered and then not staggered at the end of the animation
#### exit():
pass
#### update():
pass
#### physics_update():
pass
#### \_on\_animation\_ended():
Transition to walk
#### Transitions to
- Walk when stagger ends
### Dead
Condition:  HP <= 0
Behavior: Die; restart level, game over screen, etc.
#### enter():
Die
#### exit():
pass
#### update():
pass
#### physics_update():
pass
#### Transitions to
- Nothing, because death restarts the level