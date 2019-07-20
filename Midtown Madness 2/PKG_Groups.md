## PKG Geometry Groups

Some geometry *files* within a PKG file have special functions. This
section describes these.

### Vehicle

Required groups

| Name   | Description              |
| ------ | ------------------------ |
| BODY   | Main body of the vehicle |
| SHADOW | Shadow under the vehicle |
| HLIGHT | Head lights              |
| TLIGHT | Tail lights              |

ReqCentroidOffsetGeom

| Name | Description       |
| ---- | ----------------- |
| WHL0 | Front left wheel  |
| WHL1 | Front right wheel |
| WHL2 | Rear left wheel   |
| WHL3 | Rear right wheel  |

ReqPivotGeom

| Name   | Description |
| ------ | ----------- |
| SHOCK0 | Unknown     |
| SHOCK1 | Unknown     |
| SHOCK2 | Unknown     |
| SHOCK3 | Unknown     |

ReqBound

  - BOUND: Collision object; not used in all vehicles.

LOD control

| LOD      | Name   |
| -------- | ------ |
| High     | BODY   |
| High     | SHADOW |
| High     | WHL0   |
| High     | WHL1   |
| High     | WHL2   |
| High     | WHL3   |
| High     | SHOCK0 |
| High     | SHOCK1 |
| High     | SHOCK2 |
| High     | SHOCK3 |
| Med      | BODY   |
| Med      | SHADOW |
| Low      | BODY   |
| Low      | SHADOW |
| Very Low | BODY   |

Toggling groups Toggle.Left Headlight=HLIGHT0 Toggle.Right
Headlight=HLIGHT1 Toggle.Left Tail Light=TLIGHT0 Toggle.Right Tail
Light=TLIGHT1

#### AmbientVehicle

(Subclass of Vehicle)

Required groups

| Name    | Description       |
| ------- | ----------------- |
| SLIGHT0 | Left turn signal  |
| SLIGHT1 | Right turn signal |

ReqCentroidOffsetGeom

| Name       | Description     |
| ---------- | --------------- |
| HEADLIGHT0 | Left headlight  |
| HEADLIGHT1 | Right headlight |

ReqAmbient=ALL

Toggling groups Toggle.Left Turn Signal=SLIGHT0 Toggle.Right Turn
Signal=SLIGHT1

##### AmbientSixWheel

(Subclass of AmbientVehicle)

ReqCentroidOffsetGeom

| Name | Description |
| ---- | ----------- |
| WHL4 | Unknown     |
| WHL5 | Unknown     |

#### PlayerVehicle

(Subclass of Vehicle)

| Name   | Description   |
| ------ | ------------- |
| RLIGHT | Reverse light |

Optional groups

| Name   | Description |
| ------ | ----------- |
| BLIGHT | Brake light |
| SIREN0 | Left siren  |
| SIREN1 | Right siren |

ReqCentroidOffsetGeom

| Name           | Description                                                     |
| -------------- | --------------------------------------------------------------- |
| BREAK0         | Broken representation of the area around the front left? wheel  |
| BREAK1         | Broken representation of the area around the front right? wheel |
| BREAK2         | Broken representation of the area around the rear left? wheel   |
| BREAK3         | Broken representation of the area around the rear right? wheel  |
| BREAK01        | Broken representation of the area at the front                  |
| BREAK12        | Broken representation of the area at the right?                 |
| BREAK23        | Broken representation of the area at the back                   |
| BREAK03        | Broken representation of the area at the left?                  |
| HUB0           | Unknown                                                         |
| HUB1           | Unknown                                                         |
| HUB2           | Unknown                                                         |
| HUB3           | Unknown                                                         |
| TRAILER_HITCH | Hook for trailer                                                |
| SRN0           | Unknown                                                         |
| SRN1           | Unknown                                                         |
| SRN2           | Unknown                                                         |
| SRN3           | Unknown                                                         |
| HEADLIGHT0     | Left headlight                                                  |
| HEADLIGHT1     | Right headlight                                                 |

Toggling groups Toggle.Reverse Lights=RLIGHT Toggle.Brake Lights=BLIGHT

Required additional files

| Name                     | Description |
| ------------------------ | ----------- |
| tune/%s.vehcarsim        | Unknown     |
| tune/%s_pov.campovcs    | Unknown     |
| tune/%s_far.camtrackcs  | Unknown     |
| tune/%s_near.trackcamcs | Unknown     |

##### Roadster

(Subclass of PlayerVehicle)

ReqCentroidOffsetGeom

| Name  | Description                       |
| ----- | --------------------------------- |
| FNDR0 | Front right? fender. Follows WHL0 |
| FNDR1 | Front left? fender. Follows WHL1  |

LOD Control

| LOD  | Name  |
| ---- | ----- |
| High | FNDR0 |
| High | FNDR1 |
| Med  | FNDR0 |
| Med  | FNDR1 |

##### MonsterTruck

(Subclass of PlayerVehicle)

Required groups

| Name     | Description |
| -------- | ----------- |
| VPTAXLE0 | Unknown     |
| VPTAXLE1 | Unknown     |

LOD Control

| LOD  | Name     |
| ---- | -------- |
| High | VPTAXLE0 |
| High | VPTAXLE1 |

ReqBound

  - BOUND: Unknown

##### BajaBuggy

(Subclass of PlayerVehicle)

ReqPivotGeom

| Name   | Description |
| ------ | ----------- |
| ARM0   | Unknown     |
| ARM1   | Unknown     |
| ARM2   | Unknown     |
| ARM3   | Unknown     |
| SHAFT2 | Unknown     |
| SHAFT3 | Unknown     |

LOD Control

| LOD  | Name   |
| ---- | ------ |
| High | ARM0   |
| High | ARM1   |
| High | ARM2   |
| High | ARM3   |
| High | SHAFT2 |
| High | SHAFT3 |
| Med  | ARM0   |
| Med  | ARM1   |
| Med  | ARM2   |
| Med  | ARM3   |
| Med  | SHAFT2 |
| Med  | SHAFT3 |

##### TrophyTruck

(Subclass of PlayerVehicle)

ReqPivotGeom

| Name  | Description |
| ----- | ----------- |
| ARM0  | Unknown     |
| ARM1  | Unknown     |
| ARM2  | Unknown     |
| ARM3  | Unknown     |
| AXLE1 | Unknown     |

LOD Control

| LOD  | Name  |
| ---- | ----- |
| High | ARM0  |
| High | ARM1  |
| High | AXLE1 |
| Med  | ARM0  |
| Med  | ARM1  |
| Med  | AXLE1 |

##### MassiveTruck

(Subclass of PlayerVehicle)

ReqPivotGeom

| Name  | Description |
| ----- | ----------- |
| ARM0  | Unknown     |
| ARM1  | Unknown     |
| ARM2  | Unknown     |
| ARM3  | Unknown     |
| AXLE0 | Unknown     |
| AXLE1 | Unknown     |

LOD Control

| LOD  | Name  |
| ---- | ----- |
| High | ARM0  |
| High | ARM1  |
| High | AXLE1 |
| Med  | ARM0  |
| Med  | ARM1  |
| Med  | AXLE1 |

##### SixWheeler

(Subclass of PlayerVehicle)

ReqCentroidOffsetGeom

| Name | Description                                                          |
| ---- | -------------------------------------------------------------------- |
| WHL4 | Far back left wheel. Imitates WHL2, in all aspects except location.  |
| WHL5 | Far back right wheel. Imitates WHL3, in all aspects except location. |

##### MoonRover

(Subclass of PlayerVehicle)

ReqCentroidOffsetGeom

| Name  | Description |
| ----- | ----------- |
| FNDR0 | Unknown     |
| WHL4  | Unknown     |
| WHL5  | Unknown     |

LOD Control

| LOD  | Name  |
| ---- | ----- |
| High | FNDR0 |

### Trailer

ReqGeom

| Name    | Description                  |
| ------- | ---------------------------- |
| TRAILER | Main body of the trailer     |
| SHADOW  | The shadow under the trailer |
| TLIGHT  | Tail lights                  |

ReqCentroidOffsetGeom

| Name           | Description       |
| -------------- | ----------------- |
| TWHL0          | Front left wheel  |
| TWHL1          | Front right wheel |
| TWHL2          | Rear left wheel   |
| TWHL3          | Rear right wheel  |
| TRAILER_HITCH |                   |

ReqBound

  - BOUND: Unknown

Toggling groups Toggle.Left Tail Light=TLIGHT0 Toggle.Right Tail
Light=TLIGHT1

#### TrailerSixWheel

(Subclass of Trailer)

ReqCentroidOffsetGeom

| Name  | Description |
| ----- | ----------- |
| TWHL4 | Unknown     |
| TWHL5 | Unknown     |

### CableCar

Required groups

| Name   | Description              |
| ------ | ------------------------ |
| BODY   | Main body of the vehicle |
| SHADOW | Shadow under the vehicle |

### Subway

Required groups

| Name   | Description              |
| ------ | ------------------------ |
| BODY   | Main body of the vehicle |
| HLIGHT | Headlights               |

ReqBound

  - BOUND: Unknown

### Prop

ReqBanger=ALL : this bad boy does it all ResetTransforms=YES

OptGeom

| Name | Description   |
| ---- | ------------- |
| GLOW | Glowing parts |

OptBound

  - BOUND: Unknown

GeomPattern

  - BREAK\*: Unknown

Table=Props

#### TrafficLight

(Subclass of Prop)

ReqBangerGlow

| Name            | Description |
| --------------- | ----------- |
| REDGLOWDAY      | Unknown     |
| YELLOWGLOWDAY   | Unknown     |
| GREENGLOWDAY    | Unknown     |
| WALK_DAY       | Unknown     |
| NOWALK_DAY     | Unknown     |
| REDGLOWNIGHT    | Unknown     |
| YELLOWGLOWNIGHT | Unknown     |
| GREENGLOWNIGHT  | Unknown     |
| WALK_NIGHT     | Unknown     |
| NOWALK_NIGHT   | Unknown     |

### Landmark

ReqCentroidOffsetGeom

| Name | Description |
| ---- | ----------- |
| ALL  | Unknown     |

ResetTransforms=YES

ReqLandmarkBound

  - BOUND: Unknown

Lines=JOIN Table=Landmarks

### Wall

ReqGeom=ALL ResetTransforms=YES OptBound=BOUND Lines=JOIN Table=Walls
TableField=Type TableValue=Custom

### Dashboard

  - The geom is created by the centroid line.
    ReqGeom=damage_needle dash gear_indicator roof speed_needle
    tach_needle
    FixMAX=NO

ReqCentroidOffsetGeom

| Name            | Description                                 |
| --------------- | ------------------------------------------- |
| damage_needle  | Needle indicating damage level              |
| dash            | Base of the dash board                      |
| gear_indicator | Gear indicator                              |
| roof            | Top section, showing the ceiling of the car |
| speed_needle   | Speed needle                                |
| tach_needle    | Tachyometer needle                          |
| wheel           | Steering wheel                              |

OptGeom

| Name        | Description |
| ----------- | ----------- |
| dash_extra | Unknown     |

ResetTransforms=YES FVF=VT1

### Hud

ReqGeom=ALL ResetTransforms=YES FVF=VT1

### Generic

ReqGeom=ALL ResetTransforms=YES

#### CPVGeneric

(Subclass of Generic)

FVF=VT1

### PedAnimations

ReqPedAnim=ALL Table=Animation

### PedModel

ReqPedMod=ALL

  - HACK so that shaders are saved out

ReqGeom=ALL Table=Animation

### Gizmo

ReqGeom=ALL

  - ReqBanger=All

ReqBound=BOUND

  - GeomPattern=BREAK\*

ResetTransforms=YES

#### GizmoBridge

(Subclass of Gizmo)

RequiredCentroidOffsetGeom

  - pivot: Unknown