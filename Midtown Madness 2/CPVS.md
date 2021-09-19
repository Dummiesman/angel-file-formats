## Introduction

MM2 looks for and reads a CPVS file when a city is loaded. The CPVS file
is used to speed up the rendering process of MM2. PVS stands for
"Potentially Visible Sets" and this is a general algorithm for grouping
objects in a certain way to make it easy for the renderer to determine
what parts of a scene to take into account when rendering.

## Format description

The CPVS file simply contains a list of visible blocks for every block
in the [PSDL](PSDL.md). What makes the CPVS file a bit different
is that the lists are compressed using a "run-length-encoding"
algorithm.

In a pseudo C-style format, the file looks like this:

```C
struct CPVS
{
    char[4] header = "PVS0";
    ulong nBlocks;    // Number of blocks + 2
    ulong[nBlocks - 2] offsets;
    uchar[rest of file] pvsLists;
}
```

The *offsets* array indicates how far into the *pvsLists* block to go to
find the start of the compressed list for each block.

The *pvsLists* array contains all compressed PVS lists in block order. A
PVS list is simply a list containing two bits for every block in the
PSDL. There is one list for each block and they are stored consecutively
in the *pvsLists* array of bytes.

Each byte of a PVS list indicates the visibility status of four blocks.
The order of the blocks in the byte is a bit awkward. Bits 0-1 are the
first block, bits 2-3 are the second, 4-5 the third and finaly bits 6-7
are the fourth block. This may sound logical, but consider that the bits
come in this order: 76543210

The values of the two bits means this:

  - 00 - Block is not visible
  - 01 - Unknown (No apparent effect)
  - 10 - Unknown (No apparent effect)
  - 11 - Block is visible

Another peculiarity with these lists is that the first two bits of every
list is reserved for something unknown. So the status for block 0 is set
in bits 2-3 of the first byte in the list.

So, consider a city consisting of 16 blocks. If blocks number 0, 2, 3
and 4 are visible from a particular block, the list of that block would
look like this:

```
Hex:   c   c    0   f    0   0    0   0    0   0
Bin:  |11001100|00001111|00000000|00000000|00000000|
Block: 2 1 0 *  6 5 4 3  a 9 8 7  e d c b  * * * f
```

Note that the last byte is padded with zero to fill up the byte.

Before the lists are stored, however, they are individually compressed
using a run-length-encoding algorithm. This is a simple compression
algorithm, it compresses sequences of repeating bytes and stores them as
a counter and a single byte instead of repeating them. So, the first
byte of a compressed sequence is a seven bit counter, n and a one-bit
operation code. The operation code, usually the top bit, bit 7,
indicates one of two operations required to decompress the next segment
of data. If the operation code is 0, i.e. the top bit is cleared, the
decoder should read the next byte and duplicate it n times. If, instead,
the operation code is 1, the decoder should read the following n bytes
as they are. After this it reads a new counter and operation code and
repeats the process.

A run-length decoder can look something like this:

```C
char buf[bufSize];
int bufPos = 0;
int i;
while (bufPos < listLen)
{
    char n = read();
    if (n &0x7f == 0)
    {
        char c = read();
        for (i = 0; i < n + 1 & 0x7f; i++)
        {
            buf[bufPos++] = c;
        }
    }
    else
    {
        for (i = 0; i < n + 1; i++)
        {
            buf[bufPos++] = read();
        }
    }
}
```

Where *bufSize* is large enough to hold the decompressed data and
*listLen* is the compressed length of the list.

The compressed representation of the example list from above looks like
this: (In hex)

`81 cc 0f 02 00`

You probably noticed that this compressed representation actually
requires the exact same number of bytes as the uncompressed list. This,
however, depends only on the fact that the example city we used has very
few blocks. In a normal city, with hundreds of blocks, the gain is
considerable. Remember that each list holds two bits for every block in
the city and most of these blocks will not be seen from any single
block. This is likely to result in large series of zeros in the lists,
these will effectively be eaten up by the run-length-encoding.
