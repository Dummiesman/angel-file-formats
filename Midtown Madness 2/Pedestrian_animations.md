## NEEDS REWRITE. This format is specification is incorrect, and is also based on the beta version of the game.

Each animation sequence, associated with a state, is made up by a number
of key frames. Each key frame has several parameters that should be
applied to each bone in the skeleton. By moving the skeleton, the entire
model will move.

## The format

The files are binary files, they start with a header containing some
parameters:

  - Number of channels
    Unsigned long integer, defining the number of channels to animate.
    It might be possible to animate more than just the motion of the
    bones. I have only seen one channel used though.
  - Number of frames
    Unsigned long integer, defines the number of key frames in this
    sequence.

After this header comes a list of all channels to animate, each channel
has a header of it's own:

  - Number of floats per frame
    Unsigned long integer defining how many floats make up each frame in
    this channel.
  - Unknown
    Float of unknown purpose.
  - Channel type
    The type of the channel, I have only seen 0x01 here, but other
    values might make other things animate, texture coordinates or
    colour values for example.

After the channel header comes all key frames. For the channel of type
0x01 each key frame first contains three floats defining a translation
of the root bone. Then follows a list of three floats for each bone in
the skeleton. The floats represent roll, pitch and heading angles
respectively.

A C-style format definition looks like this:

```
struct Anim
{
    ulong nChannels;
    ulong nFrames;
    Channel channels[nChannels];
}
```

```
struct Channel
{
    ulong nFloatsPerFrame;
    float unknown;
    byte channelType;
    Frame frames[nFrames];
}
```

```
union Frame
{
    RPHFrame rphFrame;                // If channelType = 0x01
    float floats[nFloatsPerFrame];    // Other channel types
}
```

```
struct RPHFrame
{
    Vector translation; // Translation of the root bone for this frame
    RPH rph[nBones];    // Roll, Pitch and Heading angles
}
```

```
struct RPH
{
    float roll;    // Angle in radians
    float pitch;   // Angle in radians
    float heading; // Angle in radians
}
```

Note that for channel type 0x01, nFloatsPerFrame = (nBones + 1) \* 3.
