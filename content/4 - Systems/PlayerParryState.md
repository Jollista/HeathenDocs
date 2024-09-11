---
tags:
  - Script
  - State
  - Player
---
# Condition/Behavior
**Condition**: [[Input]] parry.
**Behavior**: prevent all damage within a short window.
# Inheritance
A PlayerParryState is-a [[State]].
# Variables
- `player:`[[Player (Class)]] -> a reference to the [[Player (Scene)]] instance this [[State]] is acting upon.
- `successful:bool` -> used to indicate a successful parry.
# Functions
## enter
Zero `player.velocity`, default `successful` to false, connect signals to `_on_parry_success()` and `_on_animation_finished()`.
## exit
Disconnect all connected signals.
## \_on\_parry\_success
Set `successful` to true and play successful\_parry animation.
## \_on\_animation\_finished
If just finished an unsuccessful parry, or the successful\_parry animation, `transition()`.
### Parameters
- `anim_name:StringName` -> 
## transition
Transition to [[PlayerBlockState]].
# Authors
- Code - Jollista
- Docs - Jollista