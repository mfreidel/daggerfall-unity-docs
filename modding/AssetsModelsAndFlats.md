# Models And Flats

## Prerequisites

[Daggerfall Modelling](http://www.dfworkshop.net/downloads/daggerfall-modelling/)
: Explores and exports all of Daggerfall’s 3D models, cities, and dungeons.  
Works like a visual atlas for searching and exploring anywhere in Daggerfall’s world.

[Daggerfall Imaging 2](http://www.dfworkshop.net/downloads/daggerfall-imaging/)
: Opens and exports Daggerfall’s image files, including billboard textures.

[Daggerfall Tools for Unity](http://www.dfworkshop.net/projects/daggerfall-tools-for-unity/)
: The code library that interfaces between Daggerfall’s DOS game files and Daggerfall Unity. The included Importer allow to explore specifics models and billboards (disable Billboard Batch) as well as towns and dungeons layouts.

> Your best option is to download the full source code for [Daggerfall Unity from Github](https://github.com/Interkarma/daggerfall-unity) ([video tutorial](https://www.youtube.com/watch?v=Okv_NTSg2rw)), as it contains all the tools and it allows to test models directly in game.


## Core Features

### Common

Once you have the Unity Editor running, you can import a model placing a fbx or obj file in a subfolder of  _Assets_, for example _Assets/Resources/Models_, _Assets/Resources/Flats_ or _Assets/Game/Addons/MyMod_. You can find all required information about Unity and Models [at this page](https://docs.unity3d.com/560/Documentation/Manual/HOWTO-importObject.html), but here are provided the essential parts.

* Models exported from Daggerfall Modelling are forty times bigger than they need to be in order to be used in Unity, so you need to scale them down by 0.025.
* Unity engine reads models as [Y-up not Z-up](https://docs.unity3d.com/560/Documentation/Manual/HOWTO-FixZAxisIsUp.html) so you need to address this in your modelling software. If is blender, you can use [this plugin](https://forum.unity3d.com/threads/blender-unity-rotation-fix.181870/).
* Your model can’t have more than 65,534 vertices. If needed, split your model in several parts and put them together inside a prefab. You can also include additional components such as lights and particle effects.
* If the model is supposed to collide with the player, check _Generate colliders_ in the [mesh inspector](https://docs.unity3d.com/560/Documentation/Manual/UsingTheInspector.html) inside the Unity editor.
* When you replace an exterior model, is a good idea to provide a [LOD Group](https://docs.unity3d.com/550/Documentation/Manual/class-LODGroup.html).
* Wind is natively supported for [SpeedTrees](https://docs.unity3d.com/550/Documentation/Manual/SpeedTree.html) and trees created with the [Unity Tree Creator](https://docs.unity3d.com/550/Documentation/Manual/class-Tree.html). Additionally, it can affect particle systems through the [_External Forces_ module](https://docs.unity3d.com/550/Documentation/Manual/PartSysExtForceModule.html).
* When you build a mod you only need to pack this parent mesh/prefab; children such as meshes, materials, particle effects etc. are automatically imported.
* Materials named _Archive\_Record-0_ will be affected by custom textures provided by loose files.

### Models

Model replacements included with a mod are automatically searched by filename, which must correspond to the numeric index as shown by Daggerfall Modelling.

[![](http://www.dfworkshop.net/wp-content/uploads/2017/04/models_replacement_1.jpg)](http://www.dfworkshop.net/wp-content/uploads/2017/04/models_replacement_1.jpg)

Replacements aren’t limited to static meshes. Any _GameObject_ (_Prefab_ when serialized in editor) is supported, provided that all components on the object are known by the game (the topic of custom components is addressed [here](https://www.dfworkshop.net/projects/daggerfall-unity/modding/features/#asset-loading)). For example, this means that you can include particle systems or additional lights.

There is a standard naming to support Daggerfall season and climate variations, as shown in the list below:

1.  ID
2.  ID\_ClimateSeason _(1)_

_(1) Desert has no seasons: use ID\_Desert._

**Climates**

*   Desert
*   Mountain
*   Temperate
*   Swamp

**Seasons**

*   Fall
*   Spring
*   Summer
*   Winter

The base name is used as a per-mod fallback, following load order. The base name (meaning the ID) matches everything. For example a mod that only provides base models can be extended by other mods with specific seasons and climates, while the base model is still used for the non-provided seasons and climates. But if mod with base is loaded in a lower position, load order is respected and “overrides” everything.

### Flats

![](http://www.dfworkshop.net/wp-content/uploads/2017/04/flats_replacement_0-300x150.png)]

The original game used billboards (_sprites_) to render characters, enemies, animals and small props instead of 3d models. While is possible to [only replace billboard textures](https://www.dfworkshop.net/projects/daggerfall-unity/modding/textures/#billboards), providing a full 3d replacement is also supported. Prefabs must be named **archive\_record**, as they appear from **Daggerfall Imaging**, and bundled in a mod as usual.

These models are placed in game assuming the origin is at the center of their base, while rotation on the up axis is randomized to make up for the camera-following movement of 2d billboards (this process can be customized via scripting by adding a component that extends _IObjectPositioner_).

### Mobile Person Assets

It is possible to write a _Monobehaviour_ class that handles the instantiation process for npcs graphic assets, meaning the people that wanders around towns. This component must inherit from [MobilePersonAsset](https://thelacus.github.io/daggerfall-unity-docs/api/DaggerfallWorkshop.MobilePersonAsset.html) and be added to a prefab named _MobilePersonAsset_. This component will serve as a provider for wandering npcs, replacing  [MobilePersonBillboard](https://thelacus.github.io/daggerfall-unity-docs/api/DaggerfallWorkshop.MobilePersonBillboard.html) class.

```
using UnityEngine; using DaggerfallWorkshop;
using DaggerfallWorkshop.Game.Entity;
using DaggerfallWorkshop.Game.Utility.ModSupport;

namespace MobilePersonCubeMod
{
[ImportedComponent]
public class MobilePersonCube : MobilePersonAsset
{
    MeshRenderer meshRenderer;

    public override bool IsIdle { get; set; }

    private void Awake()
    {
        meshRenderer = GetComponent<MeshRenderer>();
    }

    public override void SetPerson(Races race, Genders gender, int personVariant, bool isGuard, int personFaceVariant, int personFaceRecordId)
    {
        Vector3 size = GetSize();
        Trigger.height = size.y * 1.2f;
        Trigger.radius = size.x * 1.2f;
    }

    public override Vector3 GetSize()
    {
        return meshRenderer.bounds.size;
    }
}
}
```


## Advanced

### Object Positioner

A component named **ObjectPositioner** can be added to a prefab to fix bad placement issues. This component has a **direction** property, which is the local space oriented axis along which the asset is transitioned.

[](https://www.dfworkshop.net/wp-content/uploads/2019/04/55683532-0ac5eb00-5941-11e9-9b74-0c89bce06100.png)

Custom positioning can also be achieved with a custom component that implements the **IObjectPositioner** interface.

### Wall Prop Positioner

**WallPropPositioner** component can be used to align models, such as torches, to the nearest wall instead of default random rotation.

### DayNight

**DayNight** can be used to toggles lights/particles on a prefab at night or change the color of emission maps. See the tooltips on each attribute for more informations.

> Note: default day color for emission maps is #406E80, while default night color is #CC922D.

### Doors

#### Exteriors

Transition from exterior to interior is implemented through a collider placed in the same position as the door on the mesh. The component **CustomDoor** allows to defines door positions using triggers at a different position.

#### Interiors

There are two prefabs for doors, **DaggerfallActionDoor \[Interior\]** and **DaggerfallActionDoor \[Dungeon\]**, both of them placed inside **Assets/Prefabs/Scene**. The entire action door prefab is replaced when models with the corresponding  model ID are provided, so mod authors need to ensure that **DaggerfallActionDoor**, **DaggerfallAudioSource** and other required components (or compatible replacements) are present.
