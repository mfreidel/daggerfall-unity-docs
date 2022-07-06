# Mod Settings

Mod settings can be created or changed with the UI settings editor. Open it from the the **tools** menu and select a folder; for example `Addons/ModName/`.

[![](https://www.dfworkshop.net/wp-content/uploads/2018/09/modSettingsEditor.png)](https://www.dfworkshop.net/wp-content/uploads/2018/09/modSettingsEditor.png)

| UI Control     | C# Type                   | Description                        |
|----------------|---------------------------|------------------------------------|
| checkbox       | boolean                   | A flag that can be set or unset    |
| multiplechoice | int                       | Index of selected choice           |
| int slider     | int                       | An integer number in a range       |
| float slider   | float                     | A floating point number in a range |
| int tuple      | Tuple&lt;int, int&gt;     | Two integer numbers                |
| float tuple    | Tuple&lt;float, float&gt; | Two floating point numbers         |
| textbox        | string                    | A raw string                       |
| color picker   | Color32                   | A color                            |

## Types available for mod settings.

Settings consist of a list of options grouped inside sections. Each section has a name, an optional description and a _advanced_ flag. Each option has a name, an optional description, a type, a default value and a number of other values depending on type. The type determines how the option is proposed to the player with a in-game UI. For example a boolean is a simple checkbox while a number is drawn as a slider in the requested range.

The end result is saved in a file named _**settings.json**_ inside the current folder. This file needs to be bundled inside the mod.

Settings can be retrived in game from the mod instance:

```
var settings = mod.GetSettings();
int number = settings.GetValue<int>("section", "key");
```

It is possible to benefit of live changes to mod settings by setting the callback **Mod.LoadSettingsCallback**.

```
private void Awake()
{
    mod.LoadSettingsCallback \= LoadSettings;
    mod.LoadSettings();
    mod.IsReady \= true;
}

private void LoadSettings(ModSettings settings, ModSettingsChange change)
{
    if (change.HasChanged("Foo"))
    {
        var number \= settings.GetValue<int\>("Foo", "Number");
        var color \= settings.GetValue<Color32\>("Foo", "Color");
    }

    if (change.HasChanged("Bar", "Text"))
    {
        var text \= settings.GetValue<string\>("Bar", "Text");
    }
}
```

## Presets

A preset is a group of values for a given number of settings, which can be provided by the mod developer or created by players and shared with the community. Presets are identified by a title and a description.

*   Presets created from the mod settings editor are saved in a file named **_presets.json_** inside the current folder. This file needs to be bundled inside the mod.
*   Presets created from the in-game UI are saved to _PersistentDataPath/Mods/GameData/GUID/modpresets.json_ (_EditorData_ when testing with the Unity Editor). Previously they were stored  in a file named **_modFileName\_presets.json_**, where **_modFileName_** is the name excluding the extension of **_modFileName.dfmod_**, inside the folder _StreamingAssets/Mods_. Existing presets are moved automatically to new position.
*   Presets can also be created by players and shared; the same functionality allows mod developers to provide presets for other mods by placing json files inside _StreamingAssets/Presets/TargetModFileName_ or _%ModRoot%/Assets/Presets/TargetModFileName_.  Read the **Share Presets** section below for more details.
*   Legacy support is currently also provided for files named **_modFileName\_presets\_name.json_**, where **_name_** is an unique identifier, and placed in the same folder of second point. Such file contains a list of one or more presets, each one with a title and description. This functionality is deprecated and will be eventually removed.


### Share presets

The presets creator window has a button labelled **Export**. When you click on it the selected preset is exported to _%PersistentDataPath%/Mods/ExportedPresets/%Mod__FileName%_ (i.e. _C:/Users/ExampleUserName/AppData/Local/Daggerfall Workshop/Daggerfall Unity/Mods/ExportedPresets/ExampleModName/ExamplePresetName.json_).

Presets can be imported from _StreamingAssets/Presets/%ModFileName%/_ (or _%ModRoot%/Assets/Presets/%ModFileName%/_ from a mod). For example _StreamingAssets/Presets/ExampleMod/ExamplePreset.json_.


## Local settings

Local mod settings are stored inside _PersistentDataPath/Mods/GameData/GUID/modsettings.json_ (_EditorData_ when testing with the Unity Editor). Previously, they were saved alongside mod files in the folder _StreamingAssets/Mods_ with the name **_modFileName.json_**, where **_modFileName_** is the name excluding the extension of _**modFileName.dfmod**_. Existing settings are moved automatically to new position.

## Versioning

Mod settings can provide a version number which is used to validate presets and
local settings. Outdated presets can still be used but a warning is shown in the
preset selection list, while local settings are deleted and replaced with a
fresh, updated, copy. Mod settings version is different than mod version becausea mod can benefits of updates that don’t require wiping local settings.
