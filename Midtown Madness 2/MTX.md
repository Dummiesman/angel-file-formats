## Local objects

Some PKGs have several geometry files that are defined in a local
coordinate system. These are recognised by the fact that they have an
additional, external file in the filesystem. The name of this file is
*<pkgname>_<geometryname>.mtx* where *pkgname* is the filename of the
*PKG*, without the *.pkg* extension and *geometryname* is the name of
the internal geometry file excluding the trailing underscore and LOD
name. For example, the *PKG* named *vpauditt.pkg* has a locally defined
geometry file named *WHL0_H*. The *MTX* file for this is named
*vpauditt_whl0.mtx*. If the *geometryname* only contains a LOD name,
the *geometryname* is replaced by *(null)*.

What the *MTX* file does is to define the location and extent of the
geometry file in question. MM2 can manipulate the piece of geometry
based on this information in order to animate the parts of the *PKG*
object. For example, turning the wheels depending on current velocity
and steering, while maintaining quick and accurate collision detection.

### MTX Structure

The file contains four vectors. The first three define a bounding box
and a center of gravity while the fourth defines an offset for the
coordinate system.

`struct MTX`
`{`
`   Vector3D min;    // Minimum x, y and z`
`   Vector3D max;    // Maximum x, y and z`
`   Vector3D cog;    // Center of gravity, object is rotated around this point`
`   Vector3D origin; // Origin of the local coordinate system`
`}`