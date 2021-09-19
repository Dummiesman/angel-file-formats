![Pedmodel_woman.skel.png](Pedmodel_woman.skel.png
"Pedmodel_woman.skel.png") The parts of a pedestrian model are bound to
a skeleton. A skeleton is constructed by a number of bones that are
attached with joints. In MM2 the skeletons are defined using a tree
structure.

### The format

First comes a parameter defining the number of bones, or actually
joints, the skeleton is made of:

  - NumBones <int>

Then follows the tree of bones, starting with a root bone that
represents the origin of the character. Each bone starts with:

  - bone <name> {
    Where name is a string, without whitespace, naming the bone. Each
    bone ends with:
  - }

Between these comes, first an offset and then a list of bones attached
to the current bone. The offset is on this format:

  - offset <float> <float> <float>
    Where the floats are the *x*, *y* and *z* of a translation relative
    to the parent bone.

The list of bones is just new bone definitions after each other, with no
separators except whitespace.

When rendering a pedestrian model, each vertex has to be modified
according to the bone it is connected to. For example, if we have a
skeleton that looks like this:

```
NumBones 3
bone root {
   offset 0.000570 1.109821 -0.000000
   bone spine {
      offset 0.000000 0.034733 -0.000000
      bone neck {
         offset 0.000000 0.329534 -0.000000
         bone head {
            offset 0.000000 0.107686 -0.000000
         }
      }
   }
}
```

And we want to render the faces connected to the head, each vertex has
to be translated by four offsets. First the offset of the root bone,
then the offset of the spine bone, then the neck bone and at last the
head bone. So, to get the final location of each point, the skeleton
tree has to be traversed from the root to the bone that the face is
attached to. Fortunately translations are commutative operations, *a* +
*b* = *b* + *a*, so you can traverse the tree from the connected bone
and back to the root instead. This is easier to implement. An even more
effective way to render would be to construct a single transformation
matrix for each bone at load time. Then we only have to apply one
translation for each vertex.
