![Pedmodel_woman.csv.png](Pedmodel_woman.csv.png
"Pedmodel_woman.csv.png") The animation of a pedestrian is based on a
state model. A pedestrian is always in a specific state and each state
is connected to an animation sequence. For example, if the pedestrian is
in the state "WALK", the animation named pedanim_womwalk.anim is
looping over and over again. When something happens, the pedestrian
might make a transition from one state to another. For example, if the
pedestrian is about to stop, a transition from WALK to WALK_STAND is
activated. When the animation sequence associated with this state is
played a transition to the STAND state is activated. Then the animation
sequence associated with STAND is played. This state is looping, so it
plays until MM2 activates another state transition.

Some states have alternatives, for example STAND and STAND2. These two
states are associated with different animation sequences to add a little
variation to pedestrian behaviour. MM2 selects one of the states
randomly and the alternative state uses the same transitions as the
original state.

## The format

The format is a comma separated table, each row defines one state and is
constructed of the following values in this order:

  - State name - String, with no whitespace, defining the name of the
    state.
  - Animation sequence - String, the name of the animation sequence
    associated with this state. these are the padanim_\*.anim file
    names without the .anim extension.
  - First frame - Integer defining on which frame the animation (loop?)
    begins.
  - Last frame - Integer defining on which frame the animation (loop?)
    ends.
  - Y axis - Float, unknown. Perhaps a rotation angle.
  - Y distance - Float, unknown. Perhaps an offset.
  - Z axis - Float, unknown. Perhaps a rotation angle.
  - Z distance - Float, unknown. Perhaps an offset.
  - Next state - String, the name of the state that is activated when
    the animation sequence has played through. For a looping state, this
    indicates the current state.

The state names are defined by MM2, the MM2 AI engine calls only for
these states. However, it might be possible to create additional
transitional states, i.e. the states that are named as STATE1_STATE2.
These are made to make smooth transitions from one looping state to
another. It might be possible to add custom chains of such states
without disrupting the AI engine.