BND is a simple ASCII format file used to define the collision box of an object. An example of a BND file would be this:

<i>vp4x4_BOUND.bnd</i>

    version: 1.01     //Always seems to be 1.01?
    verts: 12     //Vertex count
    materials: 1     //How many material definitions the BOUND has
    edges: 0     //Edge count
    polys: 13     //Poly count

    v   -1.237810   0.624240    2.996410
    v   -1.237810   1.771920    2.827500
    v   -1.237810   2.202820    -0.881580
    v   -1.237810   1.310590    -2.904570
    v   -1.237810   0.420690    -2.904570
    v   1.243870    0.624240    2.996410
    v   1.243870    0.420690    -2.904570
    v   1.243870    1.310590    -2.904570
    v   1.243870    2.202820    -0.881580
    v   1.243870    1.792910    2.827500
    v   -1.237810   2.192200    1.307540
    v   1.243870    2.192200    1.307540

    mtl default {
        elasticity: 0.100000
        friction: 0.500000
        effect: none
        sound: 0
    }

    // the final number is the material index
    quad 0  1  2  3  0
    tri 3  4  0  0
    quad 5  6  7  8  0
    quad 11  9  5  8  0
    quad 0  4  6  5  0
    quad 4  3  7  6  0
    quad 2  10  11  8  0
    tri 1  0  5  0
    tri 5  9  1  0
    quad 8  7  3  2  0
    tri 9  11  10  0
    tri 10  1  9  0
    tri 1  10  2  0

BND's can have their own materials (grass, cobblestone, etc.) depending
on what the purpose of the BND is (car's collision box, drivable
surfaces, etc.)
