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
  - type = 14 (PA8): This is a palette based image. First comes a colour
    palette, a list of blue, green, red and alpha colour components.
    After the palette comes the pixels - a matrix of references to the
    palette list.
  - type = 15 (P4_MC): This is a palette based image. First comes a
    16-colour palette, a list of blue, green, red and alpha colour
    components. After the palette comes the pixels - a matrix of
    references to the palette list - each reference is a nibble.
  - type = 16 (PA4_MC): This is a palette based image. First comes a
    16-colour palette, a list of blue, green, red and alpha colour
    components. After the palette comes the pixels - a matrix of
    references to the palette list - each reference is a nibble.
  - type = 17 (RGB888): This is a non-palette based image. The image
    data is a matrix of colour values on the form red, green and blue.
  - type = 18 (RGBA8888): This is a non-palette based image. The image
    data is a matrix of colour values on the form red, green, blue and
    alpha.

Types with a suffix *_MC* has only been seen in Midnight Club. At the
moment the difference between type 1 (P8) and type 14 (PA8) is unknown.
The difference between P4_MC and PA4_MC is also unknown.

In all types, the actual pixel matrix is organized as one horizontal
line after each other, starting with the top row. Each row goes from
left to right.

The *bits* parameter is probably not only bits. It has been found that a
value of 0x04 makes the alpha channel affect reflection rather than
transparency.

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
struct TEXFile_P4_MC    // Type 15
{
    struct Header                                 header;
    struct ColourMapEntryA8[16]                   palette;
    struct Pixel4[header.width * header.height]   imageData;
}
```

```
struct TEXFile_PA4_MC    // Type 16
{
    struct Header                                 header;
    struct ColourMapEntryA8[16]                   palette;
    struct Pixel4[header.width * header.height]   imageData;
}
```
The imageData arrays shown here contains the first image. After this
array all other mip maps are stored, the width and height are halved for
every mip map.
