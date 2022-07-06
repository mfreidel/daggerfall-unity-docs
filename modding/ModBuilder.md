# Mod Builder

The Mod Builder tool allows you to configure the fundamental properties of your
mod and package it into a .dfmod file.

![Mod Builder as it appears inside Unity]( https://www.dfworkshop.net/wp-content/uploads/2016/05/editorwindow.png )


## Create

Inside the Unity Editor, go to Assets/Game/Mods and create a subfolder for your mod, then open Daggerfall Workshop > Mod Builder, choose a title and save the manifest file inside the folder from previous point (i.e. Assets/Game/Mods/Example/Example.dfmod.json).


## Build

Open Daggerfall Workshop > Mod Builder, ensure that all the required assets are listed and build the mod. Three files with .dfmod extension are created, one for each OS. Additional meta files are also generated but are not required for mod operation.

The .dfmod file can be placed inside StreamingAssets/Mods, or any subdirectory, both in game build and Unity Editor.


## Run

Mod manifest files automatically generate a virtual mod inside the Unity Editor,
meaning that all assets listed are assumed to be part of the mod even without a
physical .dfmod file. A mod inside StreamingAssets/Mods overrides its virtual
version.

These are the steps for running a mod inside the editor in virtual mode:

1. When you create a new mod with the mod builder it asks you where to save the manifest file. Pick a subfolder of Assets/Game/Mods, for example Assets/Game/Mods/EXAMPLE/EXAMPLE.dfmod.json.
1. Select all the assets that should be shipped with the mod inside the mod builder and save. You don’t need to build.
1. Launch the game with the play button and EXAMPLE will be available as a virtual mod.

If you need to check if the mod is running in virtual mode you can use the following pattern:

```
#if UNITY_EDITOR
    if (mod.IsVirtual)
        Debug.LogFormat("{0} is running in virtual mode.", mod.Title);
    else
        Debug.LogFormat("{0} is running in editor mode.", mod.Title);
#else
    Debug.LogFormat("{0} is running in release mode.", mod.Title);
#endif
```
