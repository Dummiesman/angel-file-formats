File formats used by MM2 are described under this section.

## Common formats

Several file formats are used for many things. For example, the
[PKG](PKG.md) format is used for vehicle models, props,
monuments, facades and even the hud map. This section describes several
of these common formats.

### 3D Object models (PKG)

The geometry of small 3D objects are defined by PKG files. These files
are divided into several sections, or chunks.

The PKG-files define the objects using triangle or quad facets, each
facet is grouped into a facet group that is assigned a shader. The
shader defines colour attributes and, if needed, a texture map name.
[More...](PKG.md)

### Texture maps (TEX)

Texture mapping is the most commonly used method to apply colour to an
MM2 city and the objects within it. It has been shown that MM2 can use
some variants of the Targa (TGA) file format. Because this format is a
public one, it has been the preferred texture format for new cars and
cities.

Still, most original textures are encoded in a custom file format, known
as TEX. This file format has many features, but the most significant one
is the possibility to store mip-maps for the texture. Mip-maps are
scaled copies of the original texture map. When surfaces are distant, a
smaller scale is used and the surface looks less cluttered.
[More...](TEX.md)

## Cities

### Terrain (PSDL)

Each city has one PSDL file. This file defines roads, ground surface and
buildings. In addition, the PSDL file also provides an easy way to
automatically place props along roads etc. This is particularly good for
placing street lights, parking meters and similar objects.

The geometry of a city is divided into blocks. A block is a small
section of the city, similar to the regular definition of a city block.
One difference, though, is that roads and intersections are also placed
in their own blocks.

The block structure is used for all kinds of things in the game. Most
obvious are visible surface identification, collision detection, ambient
"intelligence", etc. All of these are calculated with respect of the
block structure. [More...](PSDL.md)

### Block culling, performance (CPVS)

Each city may also have a CPVS file, this file informs MM2 of which
blocks might be visible from every other block.
[More...](CPVS.md)

### Monuments (INST)

Each city can have one INST file, the INST file describes where to place
monuments and other objects modeled in a [PKG](PKG.md) model
file. Each placement allows a selection of paint job, scale and
orientation as well as location of a PKG. [More...](INST.md)

### Ambient paths (BAI)

Each city can have one BAI file to define paths for controlling ambients
such as pedestrians and automated traffic. [More...](BAI.md)

### Props (Pathset)

A city can have any number of pathset files. These place series of props
or decals around a city. A prop is a [PKG](PKG.md) object that
usually can be knocked down or even broken, such as small trees, cones,
park benches, et.c. [More...](Pathset.md)

### Audio

Each city has ambient sound sources placed all around. The files
controlling this are mostly unexplored. [More...](City_audio.md)

### Hud map

Each city can have one special [PKG](PKG.md) containing a map of
the city, this PKG will be shown when the player turns on his hud map.
[More...](Hud_map.md)

## Vehicles

The vehicles in MM2 are [PKG](PKG.md) objects with some
specifics.

### Player vehicles

[More...](Player_vehicles.md)

### Ambient vehicles

[More...](Ambient_vehicles.md)

## Pedestrians

The file formats for pedestrians are mostly ASCII based, this implies
that Angel Studios were rushed when completing the game - there was no
time to create a binary, more efficient format for the information.

### Pedestrian skeleton (.skel)

All the parts of a pedestrian model are bound to a skeleton. A skeleton
is constructed by a number of bones that are attached with joints. In
MM2 the skeletons are defined using a tree structure.
[More...](Pedestrian_skeleton.md)

### Model (.mod)

The pedmodel_\*.mod files define the actual 3D model of a pedestrian.
It defines vertices, some material attributes and the surfaces of the
model. [More...](Pedestrian_model.md)

### Pedestrian shaders (.shaders)

The shaders are defined in exactly the same way as for PKG objects. The
pedmodel_\*.shaders files follow the specification of the PKGFileData
in the PKG file format. [More...](PKG.md#Shaders)

### Pedestrian remap (.remap)

[More...](Pedestrian_remap.md)

### Pedestrian rays (.rays)

[More...](Pedestrian_rays.md)

### Pedestrian state models (.csv)

The animation of a pedestrian is based on a state model. A pedestrian is
always in a specific state and each state is connected to an animation
sequence. For example, if the pedestrian is in the state "WALK", the
animation named pedanim_womwalk.anim is looping over and over again.
When something happens, the pedestrian might make a transition from one
state to another. [More...](Pedestrian_state_models.md)

### Pedestrian animations (.anim)

Each animation sequence, associated with a state, is made up by a number
of key frames. Each key frame has several parameters that should be
applied to each bone in the skeleton. By moving the skeleton, the entire
model will move. [More...](Pedestrian_animations.md)
