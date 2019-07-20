## Introduction

Each city has one PSDL-file. This file defines roads, ground surface and
buildings. In addition, the PSDL-file also provides an easy way to
automatically place props along roads etc. This is particularly good for
placing street lights, parking meters and similar objects.

The geometry of a city is divided into blocks. A block is a small
section of the city, similar to the regular definition of a city block.
One difference, though, is that roads and intersections are also placed
in their own blocks.

The block structure is used for all kinds of things in the game. Most
obvious are visible surface identification, collision detection, ambient
"intelligence", etc. All of these are calculated with respect of the
block structure.

## Format description

### Resources

A PSDL-file is built up in several sections. The first section defines
several lists of resources used in the other sections. The first list is
a list of vertices. All geometrical primitives, or block attributes,
reference vertices from this list. Then follows a list of floating point
heights. These are used for facades and roof attributes. After the
heights list comes a list of texture names. Like the vertex and height
lists, the geometry references textures in this list by an index.

### Blocks

The next section defines all the blocks in the city. First comes a list
defining the perimeter of the block. This is a list of vertex references
that defines a counterclockwise triangle fan surrounding the entire
block. Each vertex reference is an index into the vertex list in the
first section. Accompanied with each vertex reference is a block index.
This block index indicates that the referenced block connects to this
one at this point. To give the possibility that a vertex does not
connect this block with another, all block indices are added by one.
Zero means that no block connects at this vertex and n means that block
*n-1* connects at this vertex. If more than one block connects at the
same vertex, the vertex is repeated in the perimeter list for each of
the connecting blocks.

After the perimeter comes a list of the various geometric primitives, or
block attributes, that defines the interior of the block. Each of these
attributes has a different structure. All known attributes are described
on the [Block attributes](Block_attributes "wikilink") page. Even though
the structure differs from attribute to attribute, each attribute is
identified by an attribute id. This id is a 16 bit integer, but only the
lowest seven bits control the type of the attribute. If bit eight is
set, this attribute is the last attribute to be rendered. The MM2
renderer won't render anything after the last attribute even if there
are more attributes in the list.

After the block definitions comes a list of eight bit flags, one byte
for each block. The bits define the type of the block. Some flags are
still unknown, but this is the current interpretation:

  - 0: Unknown, this bit is never used.
    1: Subterranean, this bit is set for blocks that are located below
    ground level (e.g. subways). The echo effect will appear on this
    block.
    2: Plain, standard, ground level block.
    3: Road, all blocks with roads should have this bit set.
    4: Intersection, all blocks that serves as intersections for ambient
    traffic and pedestrians should have this bit set.
    5: Unknown, something about the bound.
    6: Warp Room, used on anything above or below ground level.
    (effect?)
    7: INST, all blocks containing INST objects should have this bit
    set.

After the list of block types comes yet another list with one value for
each block. This list is a list of indices into the file
*proprules.csv*. If a block path goes through a block, this index is the
proprule that defines how and what props are placed in each block. The
index is the line number in the text file proprules.csv and zero means
that there are no props in this block.

### Dimensions

The dimensions of the entire city are defined in a section that follows
after the prop rule definitions. This section defines a bounding box,
first the minimum *x*, *y* and *z* values of all vertices, then the
maximum. In addition to the bounding box, this section also defines a
bounding sphere by a center and a radius.

In Midnight Club, another game that uses the same or a similar graphics
engine, it appears as if this section is extended with a large number of
bounding spheres. Perhaps one for each city block. This information is
not yet confirmed.

### Props

TODO: This section was previously interpreted as road definitions with a
strong connection to BAI. But Driver has proven that this section in
fact defines paths for roadside props. The description will be updated
accordingly.

### Structure

In a pseudo-C style structure, a PSDL-file looks like this:

`struct PSDL`
`{`
`    char[4]               identifier = "PSD0";`
`    ulong                 targetSize = 2;  // Unknown`
`    ulong                 nVertices;       // Number of vertices`
`    Vertex[nVertices]     vertices;`
`    ulong                 nHeights;        // Number of heights`
`    float[nHeights]       heights;`
`    ulong                 nTextures;       // Number of textures + 1`
`    String[nTextures - 1] textures;`
`    ulong                 nBlocks;         // Number of blocks + 1`
`    ulong                 unknown0;        // Number of junctions?`
`    Block[nBlocks - 1]    blocks;`
`    char                  unknown1 = 0x00;`
`    char[nBlocks - 1]     blockType;       // List of bytes with block type flags`
`    char                  unknown2 = 0xcd;`
`    char[nBlocks - 1]     propRule;        // Identifies what prop rule to use for each`
`                                           // block, in case a prop path traverses the`
`                                           // block`
`    // Terrain dimensions:`
`    Vertex                min;             // min and max defines a bounding box around`
`                                           // the entire terrain`
`    Vertex                max;`
`    Vertex                center;          // Center of bounding sphere`
`    float                 radius;          // Radius of bounding sphere`

`    // Block based props section:`
`    ulong                 nPaths;          // Number of prop path definitions`
`    BlockPath[nPaths]     paths;`
`}`

`struct Vertex`
`{`
`    float x;`
`    float y;`
`    float z;`
`}`

`struct String`
`{`
`    uchar            length;            // Length of string including terminator`
`    char[length - 1] string;            // ASCII characters of name part of a texture`
`                                        // filename`
`    char             terminator = 0x00;`
`}`

`struct Block`
`{`
`    ulong                            nPerimeterPoints; // Number of perimeter points`
`    ulong                            attributeSize;    // Size of attribute list`
`    PerimeterPoint[nPerimeterPoints] perimeter;`
`    ushort[attributeSize]            `[`attributes`](Block_attributes "wikilink")`;`
`}`

`struct PerimeterPoint`
`{`
`    ushort vertex;  // index in vertex list`
`    ushort block;   // index in block list + 1`
`}`

`struct BlockPath // TODO: This description is wrong, it reflects a previous`
`                 // misconception about a connection to the BAI file.`
`                 // It adds up, but some of the parameter names are`
`                 // wrong.`
`{`
`    ushort                   unknown4;`
`    ushort                   unknown5;`
`    ubyte                    nFLanes;         // Number of lanes in the forward`
`                                              // direction?`
`    ubyte                    nBLanes;         // Number of lanes in the`
`                                              // backward direction?`
`    float[nFLanes + nBLanes] density;         // Traffic density on each lane?`
`    ushort                   unknown6;        // Looks like some flags in a`
`                                              // bit-field.`
`    ushort[4]                startCrossroads; // Vertices defining the road`
`                                              // part of a crossing at the`
`                                              // start of the road.`
`    ushort[4]                endCrossroads;   // Vertices defining the road`
`                                              // part of a crossing at the end`
`                                              // of the road.`
`    uchar                    nRoadBlocks;     // Number of blocks that make up`
`                                              // this road.`
`    ushort[nRoadBlocks]      roadBlocks;      // Block ID + 1 of each block`
`                                              // that makes up this road.`
`}`