# Scripting

C# scripts can be provided by a mod using one of the following methods:

- Add C# files (.cs) as text assets to the list of assets bundled with the mod; they will be compiled at runtime with a portable mcs compiler. Please note that enums, string interpolation and tuple syntax are not supported.
- Add C# files (.cs) as text assets to the list of assets bundled with the mod and enable the flag Precompiled (experimental); they will be compiled at build time in a single assembly using the same Mono compiler used for the core game.
- If you want to compile an assembly directly from Visual Studio, you can add the result as a binary asset with extension .dll.bytes to the list of assets bundled with the mod. The assembly needs to reference UnityEngine.CoreModule (Unity) and Assembly-CSharp (Daggerfall Unity).

## Static intro point example

If a mod contains scripts, a static intro point is needed to instantiate one of them and effectively start the mod.

```
namespace ExampleMod
{
    public class Example : MonoBehaviour
    {
        static Mod mod;

        [Invoke(StateManager.StateTypes.Start, 0)]
        public static void Init(InitParams initParams)
        {
            mod = initParams.Mod;
            var go = new GameObject(mod.Title);
            go.AddComponent<Example>();
        }

        void Awake()
        {
            var settings = mod.GetSettings();
            // ---

            mod.IsReady = true;
        }
    }
}
```
