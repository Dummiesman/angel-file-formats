## Introduction

Many aspects of MM2 requires geometric objects. These range from
lampposts and waste bins, through monuments and complex facades to
vehicles hudmaps and dashboards. A PKG file defines the geometric
properties of a single object as well as the visual material properties
of the surfaces.

The PKG format used in the game Midnight Club differs slightly from this
format. A description of the differences can be found
[here](PKGMC "wikilink").

## Format description

### Files

The PKG file format is built up by several chunks of data, these chunks
are called *files*. There are a number of different types of files, each
of these contain different types of data that make up the complete 3D
model of the object.

Each file has a name. This name determines the type of the file.

| Name                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| *\*VL, \*L, \*M, \*H* | Files whose name ends with one of *VL*, *L*, *M* or *H* define geometry primitives. The name suffixes define the LOD, the level of detail, of the particular part of the object. A PKG-file can have any number of separate parts, some have special significance, especially for vehicle models. The suffixes themself stand for Very Low, Low, Medium and High respectively. Look [here](PKG_Groups "wikilink") for a description of the special geometry groups.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| *shaders*             | A PKG-file always have one *shader* file. It defines the visual material properties to use for the surfaces defined in the geometry files. Shaders can be defined in one of two ways, either using floating point colour values or integer colour values. Each shader defines ambient, diffuse, specular and emissive colours. The floating point shader also specifies the shininess used to calculate highlights. Each shader can also specify one texture map. The shaders are organized in groups called *paint jobs*. When an object is placed in game, a certain paint job is requested, either by the [INST](INST "wikilink"), [Pathset](Pathset "wikilink") or [PSDL](PSDL "wikilink") for monuments and props or by user interaction for player vehicle models. Each paint job holds the same number of shaders, even if a particular shader is identical in several paint jobs. Each group of geometry primitives referens an index in to a single paint-job of the shader list. |
| *offset*              | Unknown, probably provides a way to offset every point of this model with a fixed vector. A PKG-file can only contain one offset file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| *xref*                | Reference to another PKG. Some objects are constructed by attaching several smaller objects to itself. Often these parts are *breakable*. This means that they can be broken off from the main object. Examples are awnings on facades or pieces of driveable vehicles that can break off as a result of careless driving.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

### Structure

MM2 can read two types of PKG files, PKG2 and PKG3, these differ
slightly in the structure. The following C-style format description
describes this using a *union* type.

```
struct PKG
{
    char[4]   header = "PKG3" | "PKG2";
    PKGFile[] files;
}
```

```
union PKGFile
{
    PKG2File pkg2file; // If header == "PKG2"
    PKG3File pkg3file; // If header == "PKG3"
}
```

```
struct PKG2File
{
    char[4]     header = "FILE";
    String      name;   // Name of this section
    PKGFileData data;   // Format depends of name, see below
}
```

```
struct PKG3File
{
    char[4]     header = "FILE";
    String      name;   // Name of this section
    long        length; // Length of this PKGFile in bytes
    PKGFileData data;   // Format depends of name, see below
}
```

```
struct String
{
    unsigned byte    length;     // Number of bytes in this string including
                                 // the string terminator.
    char[length - 1] characters; // ASCII characters
    char             terminator = '\0';
}
```

#### Geometry file

```
struct PKGFileData
{
    long nSections;    // Number of sections making up this LOD
    long nVerticesTot; // Total number of vertices in this LOD
    long nIndiciesTot; // Total number of indicies in this LOD
    long nSections2;   // Repetition of the number of sections?
                       // Highly unlikely, but I have no better suggestion at
                       // this time
    long fvf;          // Flags defining what components are provided with
                       // each vertex, see below
    PKGSection[nSections] sections;
}```

```
struct PKGSection
{
    ushort nStrips;       // Number of geometry strips in this section
    ushort flags;         // Unknown flags (Always 0 in MM2 PKGs)

    long shaderOffset;    // Offset into the shader list of the requested
                          // paintjob
    PKGStrip[nStrips] strips;
}
```

```
struct PKGStrip
{
    long primType;          // Determines the primitive type
    long nVertices;         // Number of vertices in this strip
    PKGVertex[nVertices] vertices;
    long nIndices;          // Number of indices making up the geometry strip
    ushort[nIndices] indices;
}
```

```
struct PKGVertex
{
    Vertex3D coordinate;         // If flags indicate coordinates
    Vector3D normal;             // If flags indicate normals
    Vertex2D textureCoordinate;  // If flags indicate texture coordinates
}
```

```
struct Vertex3D
{
    float x;
    float y;
    float z;
}
```

```
struct Vector3D
{
    float x;
    float y;
    float z;
}
```

```
struct Vertex2D
{
    float x;
    float y;
}
```

The bits of the PKGFILEData.fvf field are defined by the DirectX
flexible vertex format enumerated type. Some of the bits are as follows:

0.  D3DFVF_RESERVED0
1.  D3DFVF_XYZ - Coordinate
2.  D3DFVF_XYZRHW - Pre-transformed coordinate
3.  Unknown
4.  D3DFVF_NORMAL - Normal vector
5.  D3DFVF_RESERVED1
6.  D3DFVF_DIFFUSE - Vertex colour 0 (diffuse)
7.  D3DFVF_SPECULAR - Vertex colour 1 (specular)
8.  D3DFVF_TEX1 - One 2D texture coordinate

Other bits can be checked in the DirectX documentation or header file
d3dtypes.h.

The only known value for primType is PRIMTYPE_TRIANGLES (=3) that means
that the indices make up lists of complete, separate triangles.

#### Shaders

```
PKGFileData
{
    long shaderType; // Bit 7: Shader type
                     // Bit 0-6: Number of paint jobs
    long shadersPerPaintJob;
    PKGShader[Number of paint jobs * shadersPerPaintJob] shaders;
}
```

```
union PKGShader
{
    PKGFullShader fullShader; // If type is 0
    PKGLightShader lightShader; // If type is 1
}
```

**NOTE:** The original format for shaders was *wrong*\! Diffuse and
Ambient were reversed, and Emissive was incorrectly named Reflective.
The shader names have also been updated to try and better reflect their
uses.

```
PKGFullShader
{
    String  textureName;
    Color4f diffuse;
    Color4f ambient;
    Color4f specular;
    Color4f emissive;
    float   shininess; // Intensity of specular highlights
}
```

```
PKGLightShader
{
    String  textureName;
    Color4d diffuse;
    Color4d ambient;
    Color4d emissive;
    float   shininess; // Intensity of specular highlights
}
```

```
Color4f
{
    float red;
    float green;
    float blue;
    float alpha;
}
```

```
Color4d
{
    unsigned char red;
    unsigned char green;
    unsigned char blue;
    unsigned char alpha;
}
```

#### Offset

```
PKGFileData
{
    Vector3D offset; // Have not tested, but could be an offset added to all
                     // vertices in the object
}
```

#### XRef

```
PKGFileData
{
    long                 nReferences;
    PKGXRef[nReferences] references;
}
```

```
PKGXRef
{`
    Vector3D xAxis;
    Vector3D yAxis;
    Vector3D zAxis;
    Vector3D origin;
    char[32] name;
}
```

## MTX - Local objects

Some PKGs have several geometry files that are defined in a local
coordinate system. These are recognised by the fact that they have an
additional, external file in the filesystem. Look [here](MTX "wikilink")
for more information.

## Boundary files

MM2 use three file types for collision detection and material properties
on regular PKG objects:

  - [BND](BND "wikilink") - Simple bound mesh in a plain ASCII format.
  - [BBND](BBND "wikilink") - Binary version of .bnd
  - [TER](TER "wikilink") - Unknown, but the MM2 engine does read these
    files

It is not clear when to pick .bbnd/.ter over .bnd, but some indications
imply that objects placed with INST typically uses the binary formats.
