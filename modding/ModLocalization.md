# Localization

Localized text content can be provided by including csv-like tables with the following naming:

*   **textdatabase.txt**
*   **\[<identifier>\]textdatabase.txt** (i.e. \[en\]textdatabase.txt)
*   **\[<identifier>(<GUID>)\]textdatabase.txt** (i.e. \[fr(7e0510a9-be7d-4fd4-9c89-ed2f2b2e5fb3)\]textdatabase.txt))
*   **mod\_modname.txt** inside _StreamingAssets/Text_


## Localize Settings

The file **settings.json** bundled with a mod provides informations used by the mod manager to draw controls in the mod settings window and store values to a local file. This includes setting names and descriptions/tooltips. These text string are seked automatically in the language table using the following patterns:

```
Mod.Description
Settings.SectionName.Name
Settings.SectionName.Description
Settings.SectionName.KeyName.Name
Settings.SectionName.KeyName.Description
Presets.PresetTitle.Title
Presets.PresetTitle.Description
```

If a key is missing, the default value from **settings.json** is used. This means there is no need to provide an english table. No support is required from individual mods, this is all managed by the mod manager.

> The mod settings editor can automatically export an english table that can be given to translators for localization.


## Localize Additional Content

The Mod instance includes a method called **Localize()** which can be used to seek additional localized strings from the text table. The key can be a string or a succession of strings wich are concatenated.
