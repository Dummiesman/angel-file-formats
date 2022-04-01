## Structure

### Header

  - version: 1.09

This probably identifies the version of the file format, the pedestrians
in MM2 all use 1.09 even though the male and female models are made
slightly different.

  - verts: <integer>
    The number of vertices used by this model.
  - normals: <integer>
    The number of normal vectors used by this model.
  - colors: <integer>
    The models in MM2 only define one colour, so this value is always
    one. I'm not sure how the colours are used.
  - tex1s: <integer>
    Number of texture coordinates for texture 1.
  - tex2s: <integer>
    Number of texture coordinates for texture 2. This implies that each
    face in the model could have two blended textures - two separate
    sets of texture coordinates. Unfortunately the MM2 models always
    specify 0 here.
  - tangents: <integer>
    I don't know if tangents are used, but they are expected to be present if this is nonzero.
  - materials: <integer>
    The faces of the model are grouped by the material, or shader, they
    use. This indicates the number of such groups. This is equal to the
    number of shaders in each "paint job". See pedmodel_\*.shaders.
  - adjuncts: <integer>
    The faces of the model are joined with adjuncts, this parameter
    indicates the total number of adjuncts in this model.
  - primitives: <integer>
    This is the total number of faces in this model.
  - matrices: <integer>
    The model is mapped to a skeleton, see pedmodel_\*.skel. Each end
    point of a skeleton bone has a transformation matrix. This parameter
    defines how many such matrices this model uses. This is equal to the
    number of bones in the skeleton.

### Resources

After the header comes a list of all the vertices used in the model.
Each vertex is on the format:

  - v <float> <float> <float>
    where the floats are *x*, *y* and *z* respectively.

Then follows a list of normal vectors, they have a similar format:

  - n <float> <float> <float>
    where the floats are *x*, *y* and *z* respectively.

After the normals comes the colors:

  - c <float> <float> <float> <float>
    where the floats are *red*, *green*, *blue* and *alpha*
    respectively.

Now comes the texture coordinates, first set number one:

  - t1 <float> <float>
    where the floats are *s* and *t* respectively.

Then follows the second set:

  - t2 <float> <float>

Finally comes the tangents in two separate lists of statements:
  
  - ts <float> <float> <float>
  followed by a list of
  - tt <float> <float> <float>
  
The original MM2 models actually have one set of texture coordinates
defined, but since no texture sheet is supplied in the shaders, they are
not used in game.

### Model

Now all basic data is defined, now the file defines the various groups
of faces. These groups the faces to the shaders. The order of these
groups is important - it must match the order of the shaders defined in
the [pedmodel_\*.shaders](Pedestrian_shaders "wikilink") file. Each
group has a name constructed of a model name and a group name. A group definition starts with
this:

  - mtl <name> {

And ends with:

  - }

Between these comes several parameters:

  - adjuncts: <integer>
    or
    packets: <integer>
    This is where the definitions of pedmodel_man.mod and
    pedmodel_woman.mod differs. The former is defined using packets
    while the latter isn't. "adjuncts" defines the number of adjuncts
    that belong to this group. When using packets, the number of
    adjuncts are specified later. The packets parameter defines the
    number of packets that belong to this group.
  - primitives: <integer>
    The total number of primitives, or faces, used by this group.
  - textures: <integer>
    The number of textures used in this group, possibly limited to 0, 1
    or 2.
  - illum: <illumination type>
    Defines the type of illumination model to use for this group, possible values are "diffuse" or "emit"
  - ambient: <float> <float> <float>
    Defines the default ambient colour for this group, the floats are
    *red*, *green* and *blue* colour components respectively.
  - diffuse: <float> <float> <float>
    Defines the default diffuse colour for this group, the floats are
    *red*, *green* and *blue* colour components respectively.
  - specular: <float> <float> <float>
    Defines the default specular colour for this group, the floats are
    *red*, *green* and *blue* colour components respectively.

Since the parameters ambient, diffuse and specular will be overridden by
the shaders file, I don't really know what these are for. Perhaps they
are used if a paint job that doesn't exist is requested by the MM2
engine.

If the parameter textures isn't 0, the texture names are listed here on
this format:

  - texture: <int> <texturename>
    where the int is the index of the texture, starting with 1 and
    texturename is a string defining the filename of the texture without
    extension.

If the groups does not define any packets, the group definitions are
followed by one list of adjuncts and one list of primitives. Each
adjunct is defines like this:

  - adj <int> <int> <int> <int> <int>
    Where the ints are vertex index, normal index, colour index, texture
    coordinate index in set 1 and texture coordinate index in set 2
    respectively.

Each primitive used in the MM2 models are triangles, other AGE games support stp and str (strip and reversed strip)

  - tri <int> <int> <int>
    Where the ints are vertex indices of the three corners of the
    triangle.
  - stp <int> <list>
    Where the first int is count, followed by a list of indices.
  - str <int> <list>
    Same as above, but opposite winding

The way to discriminate which adjuncts and primitives belong to which
material group, the order and count of the adjuncts and primitives of
each group is used. If the first group has 90 adjuncts and 30 primitives
and the second group has 6 adjuncts and 2 primitives, MM2 uses the first
90 adjuncts from the adjunct list and the 30 first primitives from the
primitive list for the first material group. The following 6 adjuncts in
the adjunct list and the following 2 primitives are assigned to the
second material group and so on.

If packets are used, this is indicated by the prescense of the packet
parameter in the material groups, the adjuncts and primitives are
defined differently. Each packet has it's own adjunct and primitive
lists. A packet also has a list of matrix indices. A packet starts with:

  - packet <int> <int> <int> <optional:int> {
    Where the ints are number of adjuncts, number of primitives, number of matrices, and optionally number of reskins contained in this packet, respectively, and ends
    with:

<!-- end list -->

  - }

Within the packet is first the list of adjuncts, then the list of
primitives (with indices localized to the adjuncts in the packet) and last the list of matrix indices. The adjunct list is
different from before, each adjunct looks like this:

  - adj <int> <int> <int> <int> <int> <int>
    Where the ints are vertex index, normal index, colour index, texture
    coordinate index in set 1, texture coordinate index in set 2 and
    matrix index respectively. The matrix index is an index in the
    packet's list of matrices. The matrix list is defined like this:

<!-- end list -->

  - mtx <int> <int> ...
    Where the ints are the number of the corresponing bone in the
    skeleton.

Note that the former method, the method without packets, doesn't specify
what matrices, or bones, are connected in an obvious way. Instead two
more lists are placed last in the pedmodel_\*.mod file. These lists are
present even if the model uses packets, I don't know if it has to, but
it is in the models used in MM2. The last lists are used to assign each
vertex and normal to a bone. This means that the vertices and normals
must be grouped by bones in their respective lists. First comes a list
assigning bones to each vertex:

  - mtxv <int> <int> ...
    Where the first int is the number of vertices, counted from the
    beginning of the vertex list, that are connected to the first bone,
    the second int is the number of consequtive vertices connected to
    the second bone and so on. This list has the same number of ints as
    the number of matrices specified in the header and the sum of all
    values equals the total number of vertices in the vertex list.

After this comes a similar list assigning a bone to each normal vector.
It is defined in the same way:

  - mtxn <int> <int> ...
