# Textures

## Materials

Unity Materials can be bundled directly inside a mod. The name must be **archive\_record-frame._mat_**, where archive is the index of the original TEXTURE file (TEXTURE.XXX), for example _009\_1-0.mat_.

Alternatively, individual textures can be imported as loose files from _StreamingAssets/Textures_. They must be named **archive\_record-frame.png** for albedo and **archive\_record-frame\_MapTag.png** for others (for example _009\_1-0\_Normal.png_) where MapTag is one of the following:

*   Normal
*   Emission
*   MetallicGloss

Xml files can also be used to edit [Metallic](https://docs.unity3d.com/Manual/StandardShaderMaterialParameterMetallic.html) and [Smoothness](https://docs.unity3d.com/Manual/StandardShaderMaterialParameterSmoothness.html) parameters from loose files. Provide a text file named **archive\_record-0.xml** as shown in the example below.

```
<?xml version="1.0"?>
<properties>
    <metallic>0.5</metallic>
    <smoothness>0.5</smoothness>
</properties>
```


## Terrain

The terrain shader supports **albedo**, **normal** and **metallicgloss** maps for terrain. There are two ways to provide terrain textures:

*   Provider a **_Texture2DArray_ asset** created with _Daggerfall Tools > Texture Array Creator_ or third party tools. The asset must include all textures of a classic archive and be named after it (i.e. _XXX-TexArray_).
*   Provide all 56 **individual textures** with a mod or loose files to generate a texture array at runtime. All textures must be included and have the same resolution and format. MetallicGloss is an exception as a default map for all the missing textures will be created automatically. If you want to support older platforms (without _CopyTextureSupport.DifferentTypes_) you also need to enable _Read/Write_ flag on all textures, at the cost of higher memory usage. This flag is always enabled for loose files.


## Billboards

Billboard textures can be imported via dfmods or loose files (_StreamingAssets/Textures_). In both cases textures must be named **archive\_record-frame.png** and, when required, **archive\_record-frame\_Emission.png**. You must provide textures for all frames in a record. XML files can also be added to customize billboard material.

_**Archive\_Record-0.xml**_

```
<?xml version="1.0"?>
<info>
    <renderMode>Cutout</renderMode>
    <emission>False</emission>
    <uvX>0</uvX>
    <uvY>0</uvY>
    <scaleX>1</scaleX>
    <scaleY>1</scaleY>
</info>
```

Textures used for **exteriors billboards** (like animals or lights) are placed on a 4096×4096 atlas for each archive, so the total size of textures must not exceed this value. If you don’t provide the entire archive, imported textures are used together with vanilla without issues. XML files only support scale.

_**Archive\_Record-0.xml**_

```
<?xml version="1.0"?>
<info>
    <scaleX>1</scaleX>
    <scaleY>1</scaleY>
</info>
```

**Mobile billboards**, such as wandering npcs and enemies, requires all textures in a archive to be provided. The following xml files are supported:

_**Archive.xml**_

```
<?xml version="1.0"?>
<info>
    <renderMode>Cutout</renderMode>
    <emission>False</emission>
</info>
```

_**Archive\_Record-0.xml**_

```
<?xml version="1.0"?>
<info>
    <uvX>0</uvX>
    <uvY>0</uvY>
    <scaleX>1</scaleX>
    <scaleY>1</scaleY>
</info>
```


## UI Images

IMG and CIF/RCI replacements can be named as follow:

*   .IMG: **filename.png** (ex: MAP100I0.IMG.png) in _StreamingAssets\\Textures\\Img_
or inside a .dfmod.

*   .CIF, .RCI: **filename\_record-frame.png** (ex: INVE16I0.CIF\_3-0.png) in _StreamingAssets\\Textures\\CifRci_
or inside a .dfmod.

## Weapons

Weapons with metal variations can be named `WEAPON\*\*.CIF-Record-Frame\_MetalType`, where `MetalType` is one of following tags:

*   Iron
*   Steel
*   Elven
*   Dwarven
*   Mithril
*   Adamantium
*   Ebony
*   Orcish
*   Daedric

## Inventory Items

Inventory items with dye variations can be named **Archive\_Record-Frame\_DyeColor**, where DyeColor is one of the following tags. Items with default tint (_Chain_ and _Silver_) can be named **Archive\_Record-Frame**.

**Clothing**

*   Blue
*   Grey
*   Red
*   DarkBrown
*   Purple
*   LightBrown
*   White
*   Aquamarine
*   Yellow
*   Green


### Paperdoll

[![](https://www.dfworkshop.net/wp-content/uploads/2019/06/251_32-0_Ebony-with-Mask.png)](https://www.dfworkshop.net/wp-content/uploads/2019/06/251_32-0_Ebony-with-Mask.png)

Some paperdoll images require a mask, with full opacity on the alpha channel to mark masked area, and trasparency to mark non-masked area as shown in the picture. Maks must be named _Name\_Mask_ (where Name is the name of the main texture) and require **Read/Write Enabled** flag to be enabled if provided with a .dfmod. For more informations on paperdoll implementation see [Items Part 3 – Paper Doll](https://www.dfworkshop.net/items-part-3-paper-doll/).

The position on the paperdoll can be changed with an xml file, with the same name as the texture, providing a rect in pixels with the origin on the top-left corner. The original paperdoll has a resolution of 110×184, which is scaled eight times in Daggerfall Unity to achieve a resolution of 880×1472. The xml file should define the scale on which the rect is based, which allows the game to correctly convert the rect to the actual paperdoll scale, ensuring compatibility with future resolution changes.

```
<?xml version\="1.0"?>
<info\>
    <rect scale\="8"\>
        <x\>248</x\>
        <y\>0</y\>
        <width\>400</width\>
        <height\>344</height\>
    </rect\>
</info\>
```


## HUD

Some hud images require additional xml files as shown in the examples below.

**HUD Compass**  

```
    COMPASS.IMG.xml

<?xml version="1.0"?>
<info>
    <width>322</width>
    <height>14</height>
</info>
```

**COMPBOX.IMG.xml**

```
<?xml version="1.0"?>
<info>
    <width>69</width>
    <height>17</height>
</info>
```

*   **Crosshair**  
    Crosshair.png and Crosshair.xml in _StreamingAssets\\Textures_

```
<?xml version="1.0"?>
<info>
    <width>22</width>
    <height>22</height>
</info>
```


## Cursor

Cursor can be overriden with a texture provided by mod using **Cursor** texture type and **Read/Write** flag enabled. Import from loose files is not supported.


## Transport

Horse and cart textures are, respectively, MRED00I0.CFA and MRED01I0.CFA_._ While the original textures can’t be exported with Daggerfall Imaging, overrides can be provided inside the _CifRci_ folder (ex: _MRED00I0.CFA\_0-0.png_).


## Travel Map

High resolution overlays can be imported along with the main map texture following the naming **TRAV0I00.IMG-RegionName**. For example, the overlay for the _Ilessan Hills_ region should be named **TRAV0I00.IMG-Ilessan Hills.**

This is the list of region names:

```
"Alik'r Desert", "Dragontail Mountains", "Glenpoint Foothills", "Daggerfall Bluffs",
"Yeorth Burrowland", "Dwynnen", "Ravennian Forest", "Devilrock",
"Malekna Forest", "Isle of Balfiera", "Bantha", "Dak'fron",
"Islands in the Western Iliac Bay", "Tamarilyn Point", "Lainlyn Cliffs",
"Bjoulsae River", "Wrothgarian Mountains", "Daggerfall", "Glenpoint", "Betony",
"Sentinel", "Anticlere", "Lainlyn", "Wayrest", "Gen Tem High Rock village",
"Gen Rai Hammerfell village", "Orsinium Area", "Skeffington Wood",
"Hammerfell bay coast", "Hammerfell sea coast", "High Rock bay coast",
"High Rock sea coast", "Northmoor", "Menevia", "Alcaire", "Koegria", "Bhoraine",
"Kambria", "Phrygias", "Urvaius", "Ykalon", "Daenia", "Shalgora", "Abibon-Gora",
"Kairou", "Pothago", "Myrkwasa", "Ayasofya", "Tigonus", "Kozanset", "Satakalaam",
"Totambu", "Mournoth", "Ephesus", "Santaki", "Antiphyllos", "Bergama", "Gavaudon",
"Tulune", "Glenumbra Moors", "Ilessan Hills", "Cybiades"
```
