---
tags:
  - Script
  - State
---
# Goals
The goal of each state is to manage only its own logic and transitions to adhere to the [single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle). The State class should be treated as an abstract class. States should only ever be instantiated as the children of a [[StateMachine]], referred to in this article as the [[StateMachine]] parent.
# Inheritance
A State is-a [Node](https://docs.godotengine.org/en/latest/classes/class_node.html).
# Signals
- `transitioned(from:`[[State]]`, to:String)` -> used to signal to [[StateMachine]] parent when a State transition occurs.
# Functions
## enter
Called by the [[StateMachine]] parent when the State is transitioned to.
## exit
Called by the [[StateMachine]] parent the State is transitioned from.
## update
Called by the [[StateMachine]] parent every frame.
### Parameters
- `delta` -> seconds since the previous frame.
## physics_update
Called by the [[StateMachine]] parent every physics tick.
### Parameters
- `delta` -> seconds since the previous frame.
# Authors
- Code - Jollista
- Docs - Jollista