---
tags:
  - Player
  - Input
  - Concept
---
# Input Buffering
Input Buffering is a technique used to create more responsive controls where an invalid input is stored for a short time in case it becomes valid shortly after. [This video](https://youtu.be/ePoNO5P1Csc?si=tyWtrPXuQA6aZZaV) does a good job of explaining it and Coyote Time.
## Coyote Time
Coyote Time is a type of Input Buffering where, if the jump input is received shortly after walking off solid ground, the player is allowed to jump anyways.
# Input Map
The following are descriptions for the default input map for Heathen.
## walk_left
Used by [[Player (Scene)]] to move the Player left ([[PlayerWalkState]]).
- A
## walk_right
Used by [[Player (Scene)]] to move the Player right ([[PlayerWalkState]]).
- D
## jump
Used by [[Player (Scene)]] to have the Player Jump ([[PlayerJumpState]]).
- Space
## dodge
Used by [[Player (Scene)]] to have the Player Dodge ([[PlayerDodgeState]]).
## parry
Used by [[Player (Scene)]] to have the Player Parry ([[PlayerParryState]]) and Block ([[PlayerBlockState]]).
## attack
Used by [[Player (Scene)]] to have the Player attack ([[PlayerLightAttackState]], [[PlayerHeavyAttackState]]).
