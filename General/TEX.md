The TEX file format is a format for storing bit mapped images. This
format is made by Angel Studios for their games.

All TEX files start with a header, this header have information about
the image, such as width, height and the format of the image data. After
the header comes data that depend on the type of the image.

The *mips* value of the header indicates the number of mip maps that are
stored in this file. The mip maps are always a quarter of the size of
the previous image stored in the file.

The *type* value defines how the pixel data is stored according to this
table:

  - type = 1 (P8): This is a palette based image. First comes a colour
    palette, a list of blue, green, red and alpha colour components.
    After the palette comes the pixels - a matrix of references to the
    palette list.
  - type = 6 (A1R5G5B5): This is a non-palette based image. The image data
    is a set of 16 bit values, with 1 bit alpha, 5 bit red, 5 bit green, 5 bit blue
  - type = 8 (I8): This is a non-palette, greyscale image. The image data
    is 8 bits per pixel, representing a greyscale value.
  - type = 9 (A4I4): This is a non-palette based image. The image data
    is represented as 8 bits per pixel, with the lower 4 bits being a
    greyscale value, and the upper 4 bits being an alpha value
  - type = 10 (A8I8): This is a non-palette based image. The image data
    is represented as 16 bits per pixel, in two byte pairs, with the
    first byte being 8 bits alpha, and the second byte being 8 bits greyscale
  - type = 8 (A8): This is a non-palette image representing an alpha channel.
  - type = 14 (PA8): This is a palette based image. First comes a colour
    palette, a list of blue, green, red and alpha colour components.
    After the palette comes the pixels - a matrix of references to the
    palette list.
  - type = 15 (P4): This is a palette based image. First comes a
    16-colour palette, a list of blue, green, red and alpha colour
    components. After the palette comes the pixels - a matrix of
    references to the palette list - each reference is a nibble.
  - type = 16 (PA4): This is a palette based image. First comes a
    16-colour palette, a list of blue, green, red and alpha colour
    components. After the palette comes the pixels - a matrix of
    references to the palette list - each reference is a nibble.
  - type = 17 (RGB888): This is a non-palette based image. The image
    data is a matrix of colour values on the form red, green and blue.
  - type = 18 (RGBA8888): This is a non-palette based image. The image
    data is a matrix of colour values on the form red, green, blue and
    alpha.
  - type = 22 (DXT1): This is a non-pallete based image using BC1 block compression.
  - type = 24 (DXT3): This is a non-pallete based image using BC3 block compression.
  - type = 26 (DXT5): This is a non-pallete based image using BC5 block compression.

Types DXT1,DXT2,DXT3,A8I8,A4I4,I8 only become available starting with Midnight Club 2.
They are not available in Midnight Club: Street Racing or Midtown Madness 2.

The difference between PA and P formats is PA contains an alpha channel, where in P formats it's ignored.

In all types, the actual pixel matrix is organized as one horizontal
line after each other, starting with the top row. Each row goes from
left to right.

The *bits* parameter isn't completely known, and has some game specific values. But here's what is known:
```
enum TextureFlags
{
    ClampU = 0x01, // Works for all AGE games
    CloudShadowsLow = 0x04, // Appears to be specific to Midtown Madness 2
    CloudShadowsHigh = 0x02, // Appears to be specific to Midtown Madness 2
    ClampV = 0x10000, // Works for all AGE games
    Filtering=0x40000, // Works starting in Midnight Club 2
    MipFiltering = 0x80000 // Works starting in Midnight Club 2
}
```

The format can be described in a pseudo C-style manner:

```
struct Header
{
    ushort width;     // Width of image in pixels
    ushort height;    // Height of image in pixels
    ushort type;      // Type of image, see below
    ushort mips;      // Indicates how many mip maps this file contains
    ushort unknown;   // Unknown
    ulong  bits;      // Flags
}
```

```
struct colourMapEntryA8
{
    byte blue;
    byte green;
    byte red;
    byte alpha;
}
```

```
struct Pixel8
{
    byte colourMapIndex;  // Offset in the colour map list
}
```

```
struct Pixel888
{
    byte red;
    byte green;
    byte blue;
}
```

```
struct Pixel8888
{
    byte red;
    byte green;
    byte blue;
    byte alpha;
}
```

```
struct TEXFile_P8
{
    struct Header                                 header;
    struct ColourMapEntryA8[256]                  palette;
    struct Pixel8[header.width * header.height]   imageData;
}
```

```
struct TEXFile_PA8               `
{
    struct Header                                 header;
    struct ColourMapEntryA8[256]                  palette;
    struct Pixel8[header.width * header.height]   imageData;
}
```

```
struct TEXFile_RGB888               `
{
    struct Header                                 header;
    struct Pixel888[header.width * header.height] imageData;
}
```

```
struct TEXFile_RGBA8888
{
    struct Header                                  header;
    struct Pixel8888[header.width * header.height] imageData;
}
```

```
struct Pixel4
{
    nibble colourMapIndex;  // Offset in the colour map list
}
```

```
struct TEXFile_P4    // Type 15
{
    struct Header                                 header;
    struct ColourMapEntryA8[16]                   palette;
    struct Pixel4[header.width * header.height]   imageData;
}
```

```
struct TEXFile_PA4    // Type 16
{
    struct Header                                 header;
    struct ColourMapEntryA8[16]                   palette;
    struct Pixel4[header.width * header.height]   imageData;
}
```
The imageData arrays shown here contains the first image. After this
array all other mip maps are stored, the width and height are halved for
every mip map.
