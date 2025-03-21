# Tagged Stream (TSV)
Used throughout various AGE games

```cpp
char header[4]; // TSV0

repeat whole file:
short tagType;
long tagDataLength;
char tagData[tagDataLength];
```
