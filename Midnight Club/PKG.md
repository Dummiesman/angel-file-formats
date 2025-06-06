## Introduction

The PKG format of Midnight Club (MC) differs slightly from [the PKG
format of MM2](<../Midtown Madness 2/PKG.md>). This section proposes
an explanation of the differences.

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

```c
union PKGSection
{
  STDPKGSection     stdSection;     // If flags indicate standard sections
  CompactPKGSection compactSection; // If flags indicate compact sections
};

// Standard section - A single actual strip per strip, this is the only
//                    format used in MM2.
//
//
struct STDPKGSection
{
  ushort nStrips;                  // Number of geometry strips in this section
  ushort flags;                    // Unknown flags, bit 8 clear
  long shaderOffset;               // Offset into the shader list of the requested paintjob
  struct PKGStrip strips[nStrips];
};

// Compact section - Each strip can contain several separate strips and data
//                   types are smaller than in the standard section.
//
//
struct CompactPKGSection
{
  unsigned short nStrips;                 // Number of geometry strips in this section
  unsigned short flags;                   // Unknown flags, bit 8 set
  unsigned short shaderOffset;            // Offset into the shader list of the requested paintjob
  struct PKGCompactStrip strips[nStrips];
};

struct PKGCompactStrip
{
  unsigned short primType                      // Indicates primitive type
  unsigned short nVertices;                    // Number of vertices in this section
  struct PKGCompactVertex vertices[nVertices];
  unsigned short nIndices;                     // Number of indices making up the geometry strip
  struct PKGCompactIndex indices [nIndices];
};

struct PKGCompactIndex
{
  unsigned short index;   // Index in vertex list, bit 14 and 15 are special:
                          // bit 14: Vertex order is clockwise for this strip
                          // bit 15: Indicates the end of a strip
};

struct PKGOrientation
{
  unsigned char xAngle;  // Integer angle of unknown range, assumed 0-255
  unsigned char yAngle;  // Integer angle of unknown range, assumed 0-255
  unsigned char zAngle;  // Integer angle of unknown range, assumed 0-255
};

struct PKGCompactTex2D
{
  unsigned short x; // Fixed point value s = (x / 128f)
  unsigned short y; // Fixed point value t = (y / 128f)
};

struct PKGCompactVertex
{
  struct Vertex3D     coordinate;           // If PKGFileData.flags indicate coordinates
  struct PKGOrientation  normal;            // If PKGFileData.flags indicate normals
  unsigned long unknown;                    // If PKGFileData.flags indicate unknown
  struct PKGCompactTex2D textureCoordinate; // If PKGFileData.flags indicate texture coordinates
};
```

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

MC also adds the new primitive type `PRIMTYPE_TRIANGLESTRIP` (=4). This
means that the point indices in the strips define series of connected
triangles.
