# City Sky Definition

```
sky_dome 0 0.95 0.005
```

|Example Value|Name|Meaning|
|-----|-----|-----|
|sky_dome|Model file|The model file which will be used for the sky|
|0|Y Position|The Y coordinate of the sky|
|0.95|Y Follow Factor|A value between 0-1 which determines how tightly the sky will stick to the cameras Y coordinate|
|0.005|Rotation Rate|Rotation rate of the sky in rads/s|

The final sky position is (YPositon + (CameraY * YFollowFactor))
