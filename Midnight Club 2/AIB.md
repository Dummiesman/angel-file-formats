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
  
## Tag 0x4105 : Control
```cpp
short numTrafficLights;
short trafficLightIndices[numTrafficLights];
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

## Tag 0x4104 : Traffic Light
## Tag 0x410A : Traffic Light
Unknown what the difference between these types is

```cpp
Matrix34 matrix; (row major matrix made of of 4 Vector3 components)
short unkDataLen;
char unkData[unkDataLen];
short numNodes;
short nodeIndices[numNodes];
char flags;
```
