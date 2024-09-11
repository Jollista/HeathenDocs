---
tags:
  - Script
  - StateMachine
---
# Goals
The StateLabel displays the current state of a sibling [[StateMachine]] for debugging purposes.
# Inheritance
A StateLabel is-a [RichTextLabel](https://docs.godotengine.org/en/stable/classes/class_richtextlabel.html).
# Functions
## \_ready
On ready, connects the [[StateMachine]] sibling's `state_changed` signal to \_on\_state\_changed, updates `text` and prints.
## \_on\_state\_changed
Updates `text` and prints according to parameters.
### Parameters
- `from:String` -> the name of the [[StateMachine]] sibling's previous `current_state`
- `to:String` -> the name of the [[StateMachine]] sibling's new `current_state`
# Authors
- Code - Jollista
- Docs - Jollista