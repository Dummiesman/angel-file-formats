# AIB (AI)
AIB files are tagged stream files (described elsewhere in this repo)

For each of the individual data strucutres (e.g. node) keep a running counter. Ex. first node tag = first node in memory, etc.

## Tag 0x4100 : General Information
```cpp
short numNodes;
short numRails;
short numRoads;
short numTrafficLights;
short numControls;
```

## Tag 0x4101 : Node
```cpp
Vector3 position;
Vector3 direction;
short railId; // unsure
char nRailOut; // unsure
char flags; // unsure
```

## Tag 0x4103 : Road (Type 1)
## Tag 0x4107 : Road (Type 2)
```cpp
unsigned short hoodNameLength;
char hoodName[hoodNameLength+1];
char type; // guess
Vector3 center;
float radius;
short numRails;
short railIds[numRails];
short numNeighbors;
short neighborRoadIds[numNeighbors];

if(tag4107)
{
  short numUnkThings;
  char unkData[numUnkThings * 16];

  short numUnkThings2;
  char unkData2[numUnkThings2 * 16];
}
```

## Tag 0x4105 : Control
```cpp
short numTrafficLights;
short trafficLightIndices[numTrafficLights];
```

## Tag 0x4106 : Router Data
This tag has multiple sub tags. It's unknown what this does yet.

## Tag 0x4108 : File Metadata
```cpp
char name[32];
long unknown;
```

## Tag 0x4109 : Rails
```cpp
struct aiRail
{
  short nodeIdA;
  short nodeIdB;
  short word_04;
  short word_06;
  short word_08;
  short word_0a;
  short word_0c;
  short word_0e;
  short roadId; // unsure
  short flags; // unsure
};

aiRail rails[numRails]; // from info tag
```

## Tag 0x4104 : Traffic Light (Type 1)
## Tag 0x410A : Traffic Light (Type 2)
Unknown what the difference between these types is

```cpp
Matrix34 matrix; (row major matrix made of of 4 Vector3 components)
short unkDataLen;
char unkData[unkDataLen];
short numNodes;
short nodeIndices[numNodes];
char flags;
```
