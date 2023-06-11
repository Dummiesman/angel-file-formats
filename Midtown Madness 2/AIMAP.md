 # AIMAP
 
These files define various information about the city, and about races. Such as opponent data, police data, speed limit, etc.

### Police
The police section applies to race type aimap files, and a typical section may look like this
```
[Police]
4
vpcop	-515.073303 0.794136 -764.161438 0.000000 0 15 0.500000 50.000000
vpcop	-140.574158 2.248685 -426.436615 90.000000 0 15 0.500000 50.000000
vpcop	58.999908 -0.000300 71.631035 -110.000000 0 15 0.500000 50.000000
vpcop	-384.070313 24.455929 991.674805 0.000000 0 15 0.500000 50.000000
```

The first number is the amount of police cars, followed by a definition for each

|Example Value|Name|Meaning|
|-----|-----|-----|
|vpcop|Basename|The type of vehicle the cop will use|
|-515.07330|Spawn Position (X)|The X coordinate where the vehicle will spawn|
|0.794136|Spawn Position (Y)|The Y coordinate where the vehicle will spawn|
|-764.161438|Spawn Position (Z)|The Z coordinate where the vehicle will spawn|
|0.000000|Spawn Rotation (Degrees)|The spawn rotation of the vehicle, in degrees|
|0|Unknown/Unused|It's unknown what this value does, it may be unused. This value should be set to zero.|
|15|Flags|Flags explanation (TODO)|
|0.500000|Opponent Chase Chance|When looking for an opponent to chase, the game will draw a random number from 0-1, if the random number is lower or equal to this chance value, the cop will give chase|
|50.00000|Opponent Detection Distance|When looking for an opponent to chase, they must be closer than this distance in meters for the cop to give chase|

## Opponents
The opponents section applies to race type aimap files, and a typical section may look like this
```
[Opponent]
6
vpbug race1-a-0.opp 0.79 0 50.000000 0.700000 1 1 1 1 0 1.000000
vpbug race1-a-1.opp 0.79 0 50.000000 0.700000 1 1 1 1 0 1.000000
vpcaddie race1-a-2.opp 0.75 0 50.000000 0.700000 1 1 1 1 0 1.000000
vpbug race1-a-3.opp 0.79 0 50.000000 0.700000 1 1 1 1 0 1.000000
vpcaddie race1-a-4.opp 0.75 0 50.000000 0.700000 1 1 1 1 0 1.000000
vpbug race1-a-5.opp 0.79 0 50.000000 0.700000 1 1 1 1 0 1.000000
```

The first number is the amount of opponents, followed by a definition for each


|Example Value|Name|Meaning|
|-----|-----|-----|
|vpbug|Basename|The type of vehicle the opponent will use|
|race1-a-0.opp|Waypoint File|A file containing waypoints that the opponent will follow|
|0.79|Throttle|The maximum amount of throttle input the opponent may use. Values above 1.0 are valid|
|0|Unknown/Unused|Current knowledge indicates that this value is probably unused, and should just be set to zero|
|50.000000|Unknown|This value effects pathfinding in some way. High values in the thousands freeze the game. It's best to leave this at about 50.000000|
|0.700000|Braking Threshold|The AI will calculate how much brake it needs going into corners. If the calculated braking power is below this value, the AI will not apply the brakes|
|1|Avoid Traffic|Setting this to 1 makes the opponent avoid traffic, a setting of 0 means they will ignore traffic vehicles|
|1|Avoid Props|Setting this to 1 makes the opponent avoid props, a setting of 0 means they will ignore propss|
|1|Avoid Players|Setting this to 1 makes the opponent avoid player vehicles, a setting of 0 means they will ignore players|
|1|Avoid Opponents|Setting this to 1 makes the opponent avoid fellow opponents, a setting of 0 means they will ignore other opponent vehicles|
|0|Bad Pathfinding|This value uses a placeholder name and it's currently unknown what *exactly* this does. Setting it to 1 makes the AI pathfind in an unusual and sometimes broken way|
|1.000000|Turn Speed Multiplier|The AI calculates a safe speed for corners. This value scales what that safe speed is.|
