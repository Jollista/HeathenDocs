---
tags:
  - Script
  - StateMachine
---
The following is a design based on the StateMachine presented in [this video](https://www.youtube.com/watch?v=ow_Lum-Agbs).
# Goals
The goal of the StateMachine is to create a simple script to allow for easily customizable programmed behaviors for anything from [[Creature|Creatures]] to Music Controllers.
# Inheritance
A StateMachine is-a [Node](https://docs.godotengine.org/en/latest/classes/class_node.html).
# Variables
- `current_state:`[[State]] -> the state currently being processed. Must be set to an initial state before ready.
- `states:`[Dictionary](https://docs.godotengine.org/en/latest/classes/class_dictionary.html) -> a dictionary with references to each [[State]] child of the StateMachine gathered on [[StateMachine#_ready|_ready]].
# Signals
- `state_changed(from:String, to:String)` -> emitted when `current_state` is changed, with `from` being the name of the previous [[State]]; `to`, the name of the new [[State]].
# Functions
## \_ready
Initializes `states` and calls the `enter` function of the new `current_state` if it is non-null.
## \_process
Calls the `update` function of `current_state` if it is non-null.
### Parameters
- `delta` -> seconds since the previous frame.
## \_physics_process
Calls the `physics_update` function of `current_state` if it is non-null.
### Parameters
- `delta` -> seconds since the previous frame.
## \_on\_state\_transitioned
Called when a state emits the `transitioned` signal. Updates the `current_state` according to parameters.
### Parameters
- `from:`[[State]] -> the [[State]] that emitted the `transitioned` signal
- `to:String` -> the name of the [[State]] to transition to
# Authors
- Code - Jollista
- Docs - Jollista