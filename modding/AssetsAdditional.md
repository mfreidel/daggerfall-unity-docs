# Additional Resources

## Sound Fonts

The sound font used by Daggerfall Unity can be overridden with custom **SF2** files placed inside the folder _StreamingAssets/SoundFonts_. The current active sound font is defined by the configuration key _**SoundFont**_ under _**Audio**_ section.

```
[Audio]
SoundFont = AweROMGM.sf2
```

If the sound font is not loaded succesfully the game attempts to fallback to the internal one.


## Spell Icons

Classic spells icons can be replaced like other images used in game. Additionally, new icons can be provided with **spell icon packs**, composed of an atlas of icons and a metadata file.

Icons packs can be installed in the folder _StreamingAssets/SpellIcons_. The atlas must use **png** format and extension while metadata is in the shape of a json file with the same name of the atlas and _.txt_ extension.

```
{
    "displayName": "Example Test Pack",
    "rowCount": 2,
    "iconCount": 2,
    "filterMode": "Bilinear",
    "icons": [
        {
            "index": 0,
            "suggestedEffects": null
        },
        {
            "index": 1,
            "suggestedEffects": null
        }
     ]
}
```

Icons packs can also be provided by mods when bundled inside a directory ending with _Assets/SpellIcons_ (i.e. _Assets/Game/Mods/ExampleMod/Assets/SpellIcons_). An icon pack is composed of a texture asset (with any extension) and a metadata text asset (with the same format used for loose files) with _.json_ extension.

If a pack is removed the game fallbacks to classic icons.


## Text Fonts

> Since July 2019 (v0.10.26-alpha), fonts in Daggerfall Unity are now standard Unity TextMeshPro (TMP) fonts internally and font replacements are part of the Localization features.

Daggerfall Unity supports **Signed Distance Field** fonts and allows to provide custom overrides with the folder _StreamingAssets/Fonts_. For a guided explanation on the creation process read [Creating SDF Fonts For Daggerfall Unity](https://www.dfworkshop.net/creating-sdf-fonts-for-daggerfall-unity/).
