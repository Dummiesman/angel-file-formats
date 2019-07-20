BBND is a binary version of the [ASCII-based](BND.md "wikilink") format
used to define the boundary of [PKG](PKG.md "wikilink") objects.

```
struct BBND
{
  char version; // == 1
  long nVertices;
  long nMaterials;
  long nPolys;
  BBNDVertex   vertices[nVertices];
  BBNDMaterial materials[nMaterials];
  BBNDPolygon  polys[nPolys];
};

struct BBNDVertex
{
  float x;
  float y;
  float z;
};

struct BBNDMaterial
{
  char[32] name;
  float    elasticity;
  float    friction;
  char[32] effect;
  char[32] sound;
};

struct BBNDPolygon
{
  short indicies[4];
  short material;
};
```

Bound polygons can be quads and triangles. If a polygon is a triangle,
the last index is 0. Therefore a future exporter must never put the
vertex index 0 last in a quad polygon. All strings are fixed 32 byte
arrays with terminator byte 0x00.
