Work In Progress Document

SDL is the text equivalent of PSDL and is supported by sdlview


## Structure
The file begins with vertex and texture definitions.

Vertex definitions are as follows  
`v 0 0 0`

Texture definitions are as follows  
`td s_grass`

Following this, there are then definitions for rooms. Starting with the line.  
The actual meaning of the numbers is unknown, they seem to be added together to make the total room count.
`rooms 0 0 0`

Now we get to the room data. Rooms begin like so:  
(Optional) Type specifies room flags, same field as in PSDL  
Perimeter defines the number of perimeter points, followed by pairs of Vertex ID and Neighbor ID  
It's unknown if the ID field is optional  
```
room 1
id 1
type 1
perimeter 6   8 5  9 0  10 8  11 8  11 9  11 5
```

Next we move on to the actual SDL attributes.

## Attributes

### Textures
Textures are specified as follows:  
`tex ID`
Where ID is 0 for no texture, or 1+ to index the texture list

### Roads
`road VertCount VertIndices` ex. `road 8 0 1 2 3 4 5 6 7` 
Road index count must be a multiple of four (four per row)

### Crosswalks
`crosswalk 4 Indices` ex. `crosswalk 4 0 1 2 3`  
Crosswalks must have exactly 4 indices

### Sidewalk Strip
`sidewalk VertCount Indices` ex. `sidewalk 4 0 1 2 3`  
Sidewalk strip index count must be a multiple of two

### Alleyways
`alley VertCount VertIndices` ex. `alley 4 0 1 2 3`  
Road index count must be a multiple of two (two per row)

### Divided roads
Divided roads are structured the same way they are in PSDL, which means parts of it have to be constructed by a calculator.
`divroad VertCount DivroadAttributes Value VertexIndices` ex. `divroad 14 	2 1  69 70 71 72 73 74 75 76 77 78 79 80`  
The attributes are a packed 16 bit value encoding type, flags, and divider texture index. See the room attributes file for information.   
Value is eiter a height value in meters, or a texture tiling value for flat dividers
Divided road index count must be a multiple of six (six per row)

### Tunnels/Walls
Tunnels also match their PSDL equivalent.  
`ext ComponentCount Data`  
For roads the component count should be at least two, which would make data `Flags Height`  
The component count can be three as well, `Flags Height Unknown`  
It appears for intersections the component count should be nine `Flags Height Unknown WallBitsA WallBitsB InvertFirstA InvertFirstB InvertSecondA InvertSecondB`  
The wall bits and invert are all bit fields, see room attributes file for more information.  
As of yet I haven't been able to make these actually show up in sdlview.

### OStrip
This command appears to make a single triangle or quad with a culled triangle fan. Only 3 and 4 vertices are supported.  
`ostrip VertexCount VertexIndices`

### OFan
This command makes either a regular triangle fan if room flag 16 is not set, or a culled triangle fan if it is set.  
All vertices in an ofan have the sidewalk height offset removed (each vertex gets 0.15 subtracted from it)  
`ofan VertexCount VertexIndices`

### OFan2
This command is the same as OFan, but the vertex positions are not subtratcted by the sidewalk height offset.

### OFan3
This command is supported, but incomplete. It will subtract height from vertices like OFan, but it makes no geometry.

### AI
This command sets up an AI road for lvlAiMap. Each AI command sets up a single road.
`ai RoomCountForRoad RoomIDs Flags LeftLaneCount RightLaneCount SomeLaneData IntersectionVerts StopLightFlags`

Example:  
```
ai	19

	; room count and rooms
	1
	10
	
	; flags
	32
	
	; lanes count and some distances
	2 2
	0.25 0.25
	0.5 0.5
	
	; Intersection verts
	
	51 1
	64 8
	
	0 50
	56 62
	
	; stoplight flags
	1 1
```
