---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: Block button held.
**Behavior**: A block to prevent damage at the cost of structure.
# Inheritance
A PlayerBlockState is-a [[State]].
# Variables
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
# Functions
## enter
Sets `player.blocking` to `true`, and connects `player.animation` to `_on_animation_finished()` before playing the `player.animation`'s block animation.
## exit
Resets `player.blocking` to `true` and disconnects all connected signals.
## physics_update
- if [[Input]] dodge -> transition to [[PlayerDodgeState]]
- if **no** [[Input]] parry -> play `player.animation`'s block\_end animation
- if `!player.is_on_floor()` -> transition to [[PlayerFallState]]
## \_on\_animation\_finished
If `player.animation` just finished playing its block\_end animation -> transition to [[PlayerWalkState]].
### Parameters
- `anim_name:StringName` -> the name of the animation just finished
# Authors
- Code - Jollista
- Docs - Jollista