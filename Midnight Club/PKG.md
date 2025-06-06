## Introduction

The PKG format of Midnight Club (MC) differs slightly from the PKG
format of MM2. This section proposes an explanation of the differences.

### Geometry files

Some objects in MC use a new graphics primitive. Several separate strips
can be described in the same geometry structure. This is controlled by
bit eight in the flags of the PKGSection. This flag also controls the
data type of several attributes of the strips, presenting a more compact
way of representing the geometry.

As a part of this more compact representation, normals are not
represented by vectors. Instead Euler angles are used to give the
direction of the normal. The angles are given in three unsigned bytes as
a value between 0 and 255, yielding a full revolution in 256 steps.
(This range is not yet verified)

### Structure

`union PKGSection`
`{`
`    STDPKGSection     stdSection;     // If flags indicate standard sections`
`    CompactPKGSection compactSection; // If flags indicate compact sections`
`}`

`// Standard section - A single actual strip per strip, this is the only`
`//                    format used in MM2.`
`//`
`//`
`STDPKGSection`
`{`
`    ushort nStrips;       // Number of geometry strips in this section `
`    ushort flags;         // Unknown flags, bit 8 clear`
`    long shaderOffset;    // Offset into the shader list of the requested`
`                          // paintjob`
`    PKGStrip[nStrips] strips;`
`}`

`// Compact section - Each strip can contain several separate strips and data`
`//                   types are smaller than in the standard section.`
`//`
`//`
`CompactPKGSection `
`{ `
`    ushort nStrips;         // Number of geometry strips in this section `
`    ushort flags;           // Unknown flags, bit 8 set`
`    ushort shaderOffset;    // Offset into the shader list of the requested`
`                            // paintjob `
`    PKGCompactStrip[nStrips] strips;`
`} `

`PKGCompactStrip`
`{`
`    ushort primType         // Indicates primitive type`
`    ushort nVertices;       // Number of vertices in this section `
`    PKGCompactVertex[nVertices] vertices; `
`    ushort nIndices;        // Number of indices making up the geometry strip `
`    PKGCompactIndex[nIndices] indices;`
`}`

`PKGCompactIndex`
`{ `
`    ushort index;   // Index in vertex list, bit 14 and 15 are special:`
`                    // bit 14: Vertex order is clockwise for this strip`
`                    // bit 15: Indicates the end of a strip`
`}`

`struct PKGOrientation`
`{`
`    unsigned char xAngle;  // Integer angle of unknown range, assumed 0-255`
`    unsigned char yAngle;  // Integer angle of unknown range, assumed 0-255`
`    unsigned char zAngle;  // Integer angle of unknown range, assumed 0-255`
`}`

`PKGCompactTex2D`
`{`
`    ushort x; // Fixed point value s = (x / 128f)`
`    ushort y; // Fixed point value t = (y / 128f)`
`}`

`PKGCompactVertex`
`{`
`    Vertex3D     coordinate;           // If PKGFileData.flags indicate`
`                                       // coordinates`
`    PKGOrientation  normal;            // If PKGFileData.flags indicate normals`
`    ulong           unknown;           // If PKGFileData.flags indicate unknown`
`    PKGCompactTex2D textureCoordinate; // If PKGFileData.flags indicate texture`
`                                       // coordinates`
`}`

Midnight Club adds at least one new flag to the PKGFILEData.flags field,
the bits are defined as follows:

0.  Unknown
1.  Texture coordinates or coordinates
2.  Unknown
3.  Unknown
4.  Normal vectors
5.  Unknown
6.  Unknown value, 32 bits
7.  Unknown
8.  Coordinates or texture coordinates

All other bits are unknown at this time.

MC also adds the new primitive type PRIMTYPE_TRIANGLESTRIP (=4). This
means that the point indices in the strips define series of connected
triangles.