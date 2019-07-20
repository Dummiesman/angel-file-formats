The asBirthRule files define a particle effect. A particle effect is an
effect where many, small objects are spewed out from a point at certain
occasions. For weather effects, the particles can be rain drops, for
colliding with a parking meter, the particles may be coins.

The format is ASCII-based and consist of a block enclosed by *{* and
*}*.

The files start with a definition of the type of banger data given. None
of the MM2 files use anything but type *a*, like this:

`type: a`

Next comes the main asBirthRule block:

`asBirthRule {`
`...`
`}`

This block can contain several tag-value parameters:

| Tag           | Format            | Description                                                                          |
| ------------- | ----------------- | ------------------------------------------------------------------------------------ |
| Position      | float float float | Position where particles originate                                                   |
| PositionVar   | float float float | Variance in position, the origin is randomly modified within this range              |
| Velocity      | float float float | Velocity and direction of the particles when created                                 |
| VelocityVar   | float float float | Variance in velocity, the velocity is randomly modified within this range            |
| Life          | float             | Life-time of the particles                                                           |
| LifeVar       | float             | Variance in life-time                                                                |
| Mass          | float             | Mass of a particle                                                                   |
| MassVar       | float             | Variance in particle mass                                                            |
| Radius        | float             | Size of the invisible sphere containing all particles                                |
| RadiusVar     | float             | Variance in containing sphere                                                        |
| Drag          | float             | Velocity reduction over time                                                                              |
| DragVar       | float             | Variance in drag                                                                     |
| Damp          | float             | Percentage of retained velocity on collision.                                                                              |
| DampVar       | float             | Variance in damp                                                                     |
| DRadius       | float             | Size of the particles                                                                |
| DRadiusVar    | float             | Variance in particle size                                                            |
| DAlpha        | float             | Transparency of the particle                                                         |
| DAlphaVar     | float             | Variance in particle transparency                                                    |
| DRotation     | float             | Particle rotation speed, about what axis?                                            |
| DRotationVar  | float             | Variance in particle rotation                                                        |
| InitialBlast  | int               | Number of particles created immediately on activation                                |
| SpewRate      | float             | Number of new particles per second?                                                  |
| SpewTimeLimit | float             | Maximum time, in seconds, for particle creation                                      |
| Gravity       | float             | Gravitational factor for particles, for Earth conditions enter -9.8 or thereabaouts  |
| TexFrameStart | int               | Index of the first frame of particle animation in the image, see below               |
| TexFrameEnd   | int               | Index of the last frame of particle animation in the image , see below               |
| BirthFlags    | int               | Particle flags. 2 = Collision, 4 = Animated, 8 = KillOnCollision, 16 = Animated (cyle from start-end frame rapidly). Can be combined.                                                                              |
| Height        | float             | Unknown                                                                              |
| Intensity     | float             | Unknown                                                                              |
| Color         | int               | Controls colour of the particles in some mysterious way that only Stereo understands |

Tags of the asBirthRule block

Some of the global particle effects use the texture image named `ptx_wheel`. This image,
and all particle texture images, are divided into rectangular frames
used for animation. All frames are equal in size and they are numbered
for easy access in the asBirthRule. Animation sequences must be made up
of consequtive frames in the image, but the image may contain other
frames for different particle effects.

Oject collision particle effects may use other images, see *TexNumber*
in [dgBangerData](DgBangerData.md "wikilink"), but these are structured in
the same way, with frames and indices to the frames.
