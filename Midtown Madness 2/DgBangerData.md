The *dgBangerData* files are used to define collision properties for
[PKG](PKG.md) objects. These properties define how an object
behaves when something collides with it.

The file names are derived from the name of the [PKG](PKG.md)
and the names of the internal parts of it. The syntax is like this for
the banger data of the entire object:

* `/tune/banger/<pkgname>.dgBangerData`

and like this for the breakable parts:

* `/tune/banger/<pkgname>_<partname>.dgBangerData`

For example, the banger data for the wooden barricade of SF and London, `/geometry/sp_barricadewood_f.pkg`, is defined in the
following files:

* `/tune/banger/sp_barricadewood_f.dgBangerData`
* `/tune/banger/sp_barricadewood_f_BREAK01.dgBangerData`
* `/tune/banger/sp_barricadewood_f_BREAK02.dgBangerData`
* `/tune/banger/sp_barricadewood_f_BREAK03.dgBangerData`
* `/tune/banger/sp_barricadewood_f_BREAK04.dgBangerData`

## Specification

The format is ASCII-based and made up of tag-value rows grouped in
*blocks* between *{* and *}* symbols.

The files start with a definition of the type of banger data given. None
of the MM2 files use anything but *type a*, like this:

```
type: a
```

Next comes the main *dgBangerData* block:

```
dgBangerData {
<tags>
}
```

*<tags>* is replaced by a list of tags from the following table. The
order of the tags is significant, but some tags may be omitted from the
file, in that case the values from *default.dgBangerData* is used.

| Tag           | Format            | Description                                                                                                                                                                       |
| ------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AudioId       | int               | Index in a list of sound effects the object makes all the time?                                                                                                                   |
| Size          | float float float | Width, height and depth of the bounding box around the origin of the object                                                                                                       |
| CG            | float float float | Vector between the origin of the model and the position of the object in the world. Used to position the object.[^CG]                                                             |
| NumGlows      | int               | Number of image light glow effects listed below using the GlowOffset tag. These effects are only active at night                                                                  |
| GlowOffset    | float float float | Local offset of glows, repeated 1 .. NumGlows                                                                                                                                     |
| Mass          | float             | Mass of the object in kilograms, closely related to the objects weight                                                                                                            |
| Elasticity    | float             | Controls the *bounciness* of the object                                                                                                                                           |
| Friction      | float             | friction coefficient, determines how much force is required to slide along the surface of the object                                                                              |
| ImpulseLimit2 | float             | Force required to move the object                                                                                                                                                 |
| SpinAxis      | int               | Unknown, always set to 0 in MM2 files                                                                                                                                             |
| Flash         | int               | Unknown                                                                                                                                                                           |
| NumParts      | int               | Number of breakable parts in the object                                                                                                                                           |
| BirthRule     | block             | Particle effect for collision, see below                                                                                                                                          |
| TexNumber     | int               | Number of the particle texture map, see below                                                                                                                                     |
| BillFlags     | int               | Flags, 512=Unlit, others unknwon at this time                                                     |
| YRadius       | float             | Bounding cylinder/sphere radius                                                                                                                                                   |
| ColliderId    | int               | Index in a list of sound effects the object makes when collided with. The list is defined in the file [/aud/cardata/player/default_impacts.csv](default_impacts.csv "wikilink"). |
| CollisionPrim | int               | Defines the type of object used for collision detection, 0=boundary file, 1=bounding box, 2=cylinder with circular base, height is Y-component of *Size* and 3=sphere             |
| CollisionType | int               | Unknown, values 4, 16 and 48 are used in MM2 files                                                                                                                                |

[^CG]: The first and the last argument are mostly used for positioning breakables. Normal (undamaged) props usually use the second argument only, chiefly as half the height of the object. Contrary to earlier beliefs, this tag has nothing to do with the center of gravity or rotation.

### Tags of the dgBangerData block

The *BirthRule* block is the same as used in the
[asBirthRule](asBirthRule.md) files used to define particle
effects caused by player vehicles.

The image files for particle effects are named `fxptN.tex` where *N* is
an integer. MM2 has files from index one to index 16, excluding index
15. It is uncertain if more files can be added, but at least 15 might
be re-instated for new particle textures. See
[asBirthRule](asBirthRule.md) for more details.

*Billboarding* an object or an image means that it is always rotated so
that it faces the camera regardless of the actual orientation of the
object. The flags may apply restrictions to this, such as only allowing
rotation around the Y-axis, etc.
