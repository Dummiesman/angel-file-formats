__NOTOC__ The AR files of MM2 make up a common file tree, the files
are extracted to a, shared, virtual file system when needed. The
structure of this file system is as follows:

## Structure

<div class="FolderTree">

  - root
      - anim
        <div class="FolderTree-Content">
        ### Animated objects
        `      The `*`anim`*` folder holds definitions of the animated objects `
        `      of MM2. These are the pedestrians.`
        `     `
        </div>
          - \*.anim
            <div class="FolderTree-Content">
            #### Animation sequence
            `        Each `[`animation``
             ``sequence`](Pedestrian_animations "wikilink")`, `
            `        associated with a state, is made up by a number of key `
            `        frames. Each key frame has several parameters that should be `
            `        applied to each bone in the skeleton. By moving the `
            `        skeleton, the entire model will move.`
            `       `
            </div>
            <table>
            </table>
          - \*.csv
            <div class="FolderTree-Content">
            #### Animation state model
            `        The animation of a pedestrian is based on a `
            `        `[`state``
             ``model`](Pedestrian_state_models "wikilink")`. A pedestrian is `
            `        always in a specific state and each state is connected to an `
            `        animation sequence. For example, if the pedestrian is in the `
            `        state "WALK", the animation named pedanim_womwalk.anim is `
            `        looping over and over again. When something happens, the `
            `        pedestrian might make a transition from one state to another.`
            `       `
            </div>
            <table>
            </table>
          - \*.rays
            <div class="FolderTree-Content">
            #### Unknown: rays
            </div>
            <table>
            </table>
          - \*.shaders
            <div class="FolderTree-Content">
            #### Shader definitions
            `        The `[`shaders`](PKG#Shaders "wikilink")` are defined in exactly the same `
            `        way as for PKG objects. The pedmodel_*.shaders files follow `
            `        the specification of the PKGFileData in the `[`PKG`](PKG "wikilink")` file `
            `        format.`
            `       `
            </div>
            <table>
            </table>
          - \*.skel
            <div class="FolderTree-Content">
            #### Skeleton
            `        All the parts of a pedestrian model are bound to a `
            `        `[`skeleton`](Pedestrian_skeleton "wikilink")`.  `
            `        A skeleton is constructed by a number of bones that are `
            `        attached with joints. In MM2 the skeletons are defined using `
            `        a tree structure. `
            `       `
            </div>
            <table>
            </table>
          - \*.mod
            <div class="FolderTree-Content">
            #### Geometry model
            `        The `[`pedmodel_*.mod`](Pedestrian_model "wikilink")` files define the `
            `        actual 3D model of a pedestrian. It defines vertices, some `
            `        material attributes and the surfaces of the model.`
            `       `
            </div>
            <table>
            </table>
      - aud
        <div class="FolderTree-Content">
        ### Audio files
        </div>
        <table>
        </table>
      - bound
        <div class="FolderTree-Content">
        ### Object boundaries
        MM2 use three file types for collision detection and material
        properties on regular PKG objects, [BND](BND "wikilink"),
        [BBND](BBND "wikilink") and [TER](TER "wikilink").
        It is not clear when to pick .bbnd/.ter over .bnd, but some
        indications imply that objects placed with INST typically uses
        the binary formats.
        </div>
          - \*.bnd
            <div class="FolderTree-Content">
            #### ASCII bound
            `        Simple, ASCII-based file format defined `[`here`](BND "wikilink")`.`
            `       `
            </div>
            <table>
            </table>
          - \*.bbnd
            <div class="FolderTree-Content">
            #### Binary bound
            `        Binary version of the ASCII-based format, defined `[`here`](BBND "wikilink")`.`
            `       `
            </div>
            <table>
            </table>
          - \*.ter
            <div class="FolderTree-Content">
            #### Terrain bound
            `        Unknown format, defined `[`here`](TER "wikilink")`.`
            `       `
            </div>
            <table>
            </table>
      - city
        <div class="FolderTree-Content">
        ### City definitions
        Each MM2 city has a number of files defining the actual city,
        its geometry, monuments, traffic flows and so on. Many of the
        files that controls these things are stored in the *city*
        folder.
        </div>
        <table>
        </table>
          - <cityname>
              - audio_pathsets
                  - \*.pathset
                    <div class="FolderTree-Content">
                    #### Local city sound placement
                    </div>
                    <table>
                    </table>
              - decals.pathset
                <div class="FolderTree-Content">
                #### Decal placements
                `          Decals are flat images that can be placed on the ground `
                `          and on facades to increase the visual detail level. Decals `
                `          are placed using `[`pathset`](Pathset "wikilink")` files.`
                `         `
                </div>
                <table>
                </table>
              - props.pathset
                <div class="FolderTree-Content">
                #### Prop placements
                `          Props are, typically, small 3D objects like road cones, `
                `          park benches and similar that can be placed around the `
                `          city. Props are placed using `[`pathset`](Pathset "wikilink")` files.`
                `         `
                </div>
                <table>
                </table>
              - propdefs.csv
                <div class="FolderTree-Content">
                #### Roadside prop definitions
                `          The roads defined by the `[`PSDL`](PSDL "wikilink")` file can be `
                `          automatically be filled with props. This file defines `
                `          repetiton controls for certain props. The proprules file `
                `          controls which of these props are to be placed next in `
                `          line.`
                `         `
                </div>
                <table>
                </table>
              - proprules.csv
                <div class="FolderTree-Content">
                #### Roadside prop rules
                `          The roads defined by the `[`PSDL`](PSDL "wikilink")` file can be `
                `          automatically be filled with props. This file defines `
                `          the sequencing of the prop definitions defined in the `
                `          propdefs file. The `[`PSDL`](PSDL "wikilink")` file selects a proprule index `
                `          from this file for each road. `
                `         `
                </div>
                <table>
                </table>
          - <cityname>.aimap
            <div class="FolderTree-Content">
            #### Ambient control file
            `        This file controls what ambients are present in this city.`
            `       `
            </div>
            <table>
            </table>
          - <cityname>.bai
            <div class="FolderTree-Content">
            #### Ambient path file
            `        This file defines where and how the ambients may move in the `
            `        city. See `[`BAI`](BAI "wikilink")` for details.`
            `       `
            </div>
            <table>
            </table>
          - <cityname>.cpvs
            <div class="FolderTree-Content">
            #### Potentially visible set definition
            `        MM2 uses a PVS, or potentially visible set, algorithm to `
            `        speed up rendering of each game frame. These files contain `
            `        the pre-calculated PVS tree. See `[`CPVS`](CPVS "wikilink")` for details.`
            `       `
            </div>
            <table>
            </table>
          - <cityname>.inst
            <div class="FolderTree-Content">
            #### Stationary objects
            `        For stationary objects that require more detail than the `
            `        `[`PSDL`](PSDL "wikilink")` file can provide, the `[`INST`](INST "wikilink")` file can be used to `
            `        place `[`PKG`](PKG "wikilink")` objects throughout the city. These objects `
            `        will not move for anything.`
            `       `
            </div>
            <table>
            </table>
          - \*.ldef
            <div class="FolderTree-Content">
            #### LDEF
            </div>
            <table>
            </table>
          - <cityname>.lmap
            <div class="FolderTree-Content">
            #### Light map definition
            </div>
            <table>
            </table>
          - <cityname>.lt\*
            <div class="FolderTree-Content">
            #### LT
            </div>
            <table>
            </table>
          - materials.csv
            <div class="FolderTree-Content">
            #### Material mappings
            `        This file simply lists what material should be used together `
            `        with which image map.`
            `       `
            </div>
            <table>
            </table>
          - materials.mtl
            <div class="FolderTree-Content">
            #### Material definitions
            `        Defines material properties such as friction and possible `
            `        particle effecs that apply to a particular type of surface.`
            `       `
            </div>
            <table>
            </table>
          - <cityname>.psdl
            <div class="FolderTree-Content">
            #### City geometry
            `        The `[`PSDL`](PSDL "wikilink")` file defines the major geometry of the city. `
            `        All roads and most buildings are defined with this file.`
            `       `
            </div>
            <table>
            </table>
          - <cityname>.sky
            <div class="FolderTree-Content">
            #### Sky dome definition
            `        Defines the name of the sky dome object and the position for `
            `        the center of this dome relative to the `[`PSDL`](PSDL "wikilink")`.`
            `       `
            </div>
            <table>
            </table>
          - <cityname>.water
            <div class="FolderTree-Content">
            #### Water of death definition
            `        Defines the height of deadly water - if a vehicle goes below `
            `        this height it sleeps with the fishes. Possibly a list of `
            `        `[`PSDL`](PSDL "wikilink")` block ids can be listed for blocks that define `
            `        deadly water at any height. `
            `       `
            </div>
            <table>
            </table>
      - geometry
        <div class="FolderTree-Content">
        ### Object definitions
        `      The geometry of generic 3D objects are defined by PKG files. These are used for many things, from player vehicles to statues and complex facades.`
        `     `
        </div>
        <table>
        </table>
          - \*.pkg
            <div class="FolderTree-Content">
            #### Object geometry
            `        Actual geometry is defined in these files, definition can be found `[`here`](PKG "wikilink")`.`
            `       `
            </div>
            <table>
            </table>
          - \*.mtx
            <div class="FolderTree-Content">
            #### Object matrix
            `        Defines a transformation matrix for `*`local`*` parts of a `[`PKG`](PKG "wikilink")`. Format is defined `[`here`](MTX "wikilink")`.`
            `       `
            </div>
            <table>
            </table>
      - jpg
        <div class="FolderTree-Content">
        ### Game interface graphics
        </div>
        <table>
        </table>
      - texture
        <div class="FolderTree-Content">
        ### Game image maps
        </div>
        <table>
        </table>
          - \*.tex
            <div class="FolderTree-Content">
            #### Custom image map file
            `        The `[`TEX`](TEX "wikilink")` file format is a format for storing bit mapped images. This format is made by Angel Studios for their games.`
            `       `
            </div>
            <table>
            </table>
          - \*.tga
            <div class="FolderTree-Content">
            #### Targa image map file
            </div>
            <table>
            </table>
      - race
        <div class="FolderTree-Content">
        ### Race definitions
        </div>
        <table>
        </table>
          - <cityname>
              - \*.aimap
                <div class="FolderTree-Content">
                #### Ambient control file (Amateur)
                </div>
                <table>
                </table>
              - \*.aimap_p
                <div class="FolderTree-Content">
                #### Ambient control file (Professional)
                </div>
                <table>
                </table>
              - <racename>waypoints.csv
                <div class="FolderTree-Content">
                #### Race waypoint positioning
                </div>
                <table>
                </table>
              - <racename>data.csv
                <div class="FolderTree-Content">
                #### Race waypoint positioning
                </div>
                <table>
                </table>
              - <eventname>.csv
                <div class="FolderTree-Content">
                #### Event definition
                </div>
                <table>
                </table>
              - mm\[blitz|circuit|crash|race\]data.csv
                <div class="FolderTree-Content">
                #### Race progression definitions
                </div>
                <table>
                </table>
              - multicopwaypoints.csv
                <div class="FolderTree-Content">
                #### Cops and robbers locations
                </div>
                <table>
                </table>
              - <cityname>_rewards.csv
                <div class="FolderTree-Content">
                #### Race reward definitions
                </div>
                <table>
                </table>
              - <racename>-\[a|p\]-<n>.opp
                <div class="FolderTree-Content">
                #### Opponent definitions
                </div>
                <table>
                </table>
              - <racename>.pathset
                <div class="FolderTree-Content">
                #### Race props
                </div>
                <table>
                </table>
      - scene
        <div class="FolderTree-Content">
        ### Scene?
        </div>
        <table>
        </table>
          - modtypes.ini
            <div class="FolderTree-Content">
            #### Mod types?
            </div>
            <table>
            </table>
      - tune
        <div class="FolderTree-Content">
        ### Tuning information
        </div>
        <table>
        </table>
          - banger
              - <objectname>\[_BREAK\#\#\].dgBangerData
                <div class="FolderTree-Content">
                #### Object collision properties
                `          Defines properties controlling `[`PKG`](PKG "wikilink")` objects that has`
                `          collided with something. The format is described `[`here`](dgBangerData "wikilink")`.`
                `         `
                </div>
                <table>
                </table>
          - camera
              - <vehiclename>\[_dash\].camPovCS
                <div class="FolderTree-Content">
                #### Camera definitions
                </div>
                <table>
                </table>
              - <vehiclename>_\[NEAR|FAR\].camTrackCS
                <div class="FolderTree-Content">
                #### Camera tracking definitions
                </div>
                <table>
                </table>
          - effects
              - <effectname>.asBirthRule
                <div class="FolderTree-Content">
                #### Particle effect definition
                `          Particle effects are used in weather, vehicle effects and collisions. The format is described `[`here`](asBirthRule "wikilink")`.`
                `         `
                </div>
                <table>
                </table>
          - vehicle
              - <vehiclename>.aiVehicleData
                <div class="FolderTree-Content">
                #### Ambient vehicle data
                </div>
                <table>
                </table>
              - <vehiclename>.asNode
                <div class="FolderTree-Content">
                #### Joystick/Wheel settings
                </div>
                <table>
                </table>
              - <vehiclename>.dgTrailerJoint
                <div class="FolderTree-Content">
                #### Trailer joint information
                </div>
                <table>
                </table>
              - <vehiclename>.vehCarDamage
                <div class="FolderTree-Content">
                #### Vehicle damage control
                </div>
                <table>
                </table>
              - <vehiclename>\[_opp\].vehCarSim
                <div class="FolderTree-Content">
                #### Vehicle tuning
                </div>
                <table>
                </table>
              - <vehiclename>.vehGyro
                <div class="FolderTree-Content">
                #### Vehicle stability
                </div>
                <table>
                </table>
              - <vehiclename>.vehStuck
                <div class="FolderTree-Content">
                #### Vehicle instability
                </div>
                <table>
                </table>
              - <vehiclename>.vehTrailer
                <div class="FolderTree-Content">
                #### Vehicle trailer information
                </div>
                <table>
                </table>
          - \[rain|snow\].asBirthRule
            <div class="FolderTree-Content">
            #### Weather particle effect
            </div>
            <table>
            </table>
          - <vehiclename>.asNode
            <div class="FolderTree-Content">
            #### Vehicle control settings
            </div>
            <table>
            </table>
          - <vehiclename>_dash.asNode
            <div class="FolderTree-Content">
            #### Vehicle dashboard settings
            </div>
            <table>
            </table>
          - <cityname>.cinfo
            <div class="FolderTree-Content">
            #### City definitions
            </div>
            <table>
            </table>
          - <vehiclename>.dgTrailerJoint
            <div class="FolderTree-Content">
            #### Trailer joint settings
            </div>
            <table>
            </table>
          - <vehiclename>.info
            <div class="FolderTree-Content">
            #### Vehicle information
            </div>
            <table>
            </table>
          - menu.csv
            <div class="FolderTree-Content">
            #### Game interface menu options
            </div>
            <table>
            </table>
          - widget.csv
            <div class="FolderTree-Content">
            #### Game interface widgets
            </div>
            <table>
            </table>
          - <cityname>.mmHudMap
            <div class="FolderTree-Content">
            #### Hudmap definitions
            </div>
            <table>
            </table>
          - <vehiclename>.mmMirror
            <div class="FolderTree-Content">
            #### Vehicle rear view mirror definition
            </div>
            <table>
            </table>
          - <texturenamestem>.movie
            <div class="FolderTree-Content">
            #### Animated image sequence definition
            </div>
            <table>
            </table>
          - <vehiclename>.vehTrailer
            <div class="FolderTree-Content">
            #### Vehicle trailer information
            </div>
            <table>
            </table>

</div>