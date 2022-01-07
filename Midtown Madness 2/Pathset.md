Props can be placed using three different methods. The type of path is
defined by the type parameter. Values are as follows:

  - 0 - Single Points
  - 1 - Directed Points
  - 2 - Line strip

0 (Single Points) - it's a list of object positions, one point = one
position, you cannot control object direction. Spacing seems to be
ignored. Example: treepalm in SF (index: 6 on the props list).

1 (Directed Points) - the point list is a sequence of point pairs. The
first point is the position of the prop and the second defines the
direction of one of the local coordinate axis. Object can ONLY be
rotated around Y-axis and the angle is determined by vector between two
points. Spacing seems to be ignored. Example: sp_transbayexit_f in SF
(index: 46 on the props list)

2 (Line Strip) - the sequence is no longer a list of pairs. Instead the
points 0 and 1 defines the first line segment, just as in the previous
type, but the next line segment is defined by point 1 and 2, the next by
point 2 and 3 and so on. Each line segment is filled with props, spaced
by the spacing parameter. Example: sp_tree1_s in SF (index: 10 on the
props list)

## Structure

```
struct Pathset
{
    char[4] id = "PTH1";
    ulong nPaths;    // Number of paths in this pathset
    ulong currentPath; // The currently selected path, used in development tools
    struct Path paths[nPaths];
}

struct Path
{
    char[32] name;    // Name of object or texture, padded with 0x00
    ulong nPoints;    // Number of points in this path
    ulong unknown1;
    struct Point points[nPoints];
    uchar type;       // See definition below
    uchar spacing;    // Spacing of props placed between the points in
                      // units of 1/4 metres (spacing = (ulong)(metres * 4))
    char padding[2];
}

struct Point
{
    ulong unknown2;
    float x;
    float y;
    float z;
}
```
