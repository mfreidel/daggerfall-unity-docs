# Asset Loading

The Modding System provides a ready-to-use framework to store assets and load them at run time, which is internally based on **AssetBundles**. A mod bundle can contains any kind of asset that derives from **_UnityEngine.Object_**, including textures, meshes, sounds, shaders etc. Assets bundled with a mod can be retrieved from the **AssetBundle** with the **Mod** instance.

```

// Load and get a reference to an asset
Material mat = mod.GetAsset("assetName");

// Load and clone an asset
GameObject go = mod.GetAsset("assetName", true);

```

You can also load all assets in the background and then dispose the AssetBundle.

```
// Load assets from AssetBundle at startup
IEnumerator loadAssets = mod.LoadAllAssetsFromBundleAsync(true);
ModManager.Instance.StartCoroutine(loadAssets);

// Get a reference to individual assets when needed
Texture tex = mod.GetAsset("assetName");
```

## Components

Custom scripts added by a mod can be instantiated at runtime.

```
GameObject go = new GameObject();
go.AddComponent<Example>();
```

Alternatively they can be marked with the **ImportedComponentAttribute** and added directly to a prefab. Daggerfall Unity relies on custom serialization to support these additional classes.

```
[ImportedComponent]
public class Example : MonoBehaviour
{
}
```


## Loose Files

Loose files can be retrieved directly from script using the static methods defined inside the namespace _DaggerfallWorkshop.Utility.AssetInjection_.

```
Texture2D tex;
if (!TextureReplacement.TryImportTextureFromLooseFiles(Path.Combine("ModName", "Example"), mipMaps, encodeAsNormalMap, readOnly, out tex))
    tex = mod.GetAsset<Texture2D>("Example");
```
