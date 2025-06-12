movie files define how animated textures animate in the Angel Game Engine

They are simple files with a list of parameters. Early AGE games only support the `rate` parameter. Games built around the Midnight Club 2 era support the full list. The order of these does not matter.

## Parameters
- rate <fps> \
The rate at which textures in the sequence will animate
- loopbackwardrandom \
The texture will animate through a full loop, then loop backwards to a random frame, return to animating forward, and continue the cycle.
- random \
The texture will animate randomly through its sequence
- rock \
The animation will "rock" back and forth. Going from start to end, then end to start, and continue the cycle.

Example file:
```
rate 30
random
```
