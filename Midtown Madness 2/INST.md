Each city can have one INST file, the INST file describes where to place
monuments and other objects modeled in a PKG model file. Each placement
allows a selection of paint job, scale and orientation as well as
location of a PKG.

## Format description

The format is simply a list of components placing one PKG object each.
There are two types of components:

  - Coordinate component: Gives you complete freedom in placing the
    object - the component includes a complete coordinate system
    definition that allows for arbitrary rotation and scaling of the
    object.
    Simple component: A shorter component that offers limited options
    for scaling and rotating the placed object.

### Structure

```C

INST
{
    INSTComponent[] components;
}

INSTComponentHeader
{
    unsigned short room Index;              // Index of the room this 
                                            // package is placed on
    unsigned short modifiers;               // Modifies the appearance of 
                                            // the object, see below
    unsigned char  type;                    // Highest bit indicates 
                                            // component type, the rest is
                                            // length of "packageName"
    unsigned char[type & 0x7f] packageName; // Zero-terminated name-part of
                                            // the PKG filename
}

INSTCoordinateComponent
{`
    INSTComponentHeader header; // The type field has the top bit cleared
    Vector      xAxis;
    Vector      yAxis;
    Vector      zAxis;
    Vector      origo;
}

INSTSimpleComponent
{
    INSTComponentHeader header; // The type field has the top bit set
                                // (type >= 0x80)
    float    xDelta;
    float    zDelta;
    Vector location;
}

Vector
{
    float x;
    float y;
    float z;
}
```

The *modifiers* define what *paint job* should be used for the object,
among other things. The least significant bits of this field, currently
I use the lowest eight bits, but I have a feeling that 256 paint jobs
are not possible. Maybe this should be the least four bits instead.
Other bits are still unknown. It seems as if bit 8, 0x100, is set for
most monuments that are visible from every direction. Monuments doesn't
appear to have more than one paint job, but facades generally do.

*INSTCoordinateComponents* define a complete coordinate system used by
MM2 to convert each vertex in the PKG to city-coordinates. This is done
by defining the locations for (1, 0, 0), (0, 1, 0), (0, 0, 1) and (0, 0,
0) of PKG coordinates in city coordinates.

INSTSimpleComponents use a more compact, less flexible method of placing
geometry. The vector named *location* is the coordinate for (0, 0, 0) of
PKG coordinates in city-coordinates. These objects face the the
direction given by xDelta and zDelta. The scale is determined by the
magnitude of the vector.
