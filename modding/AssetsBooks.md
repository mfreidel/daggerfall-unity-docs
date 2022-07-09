# Books

Daggerfall books are located inside _arena2\\books_ and can be edited with **DFBOOEDT**, available as a part of **DFTools** package, found at [Uesp](https://en.uesp.net/wiki/Daggerfall:Files).


## Import books

Custom book files can be provided from loose files inside _StreamingAssets/Books_, for example _StreamingAssets/Books/BOK00000.TXT._ Alternatively, they can be bundled in a .dfmod as an asset with **.bytes** extension, for example _BOK00000.TXT.bytes._


## Declare new books

New books must be declared with a json file defined as follow:

```
[{
   "Name": "ExampleBook.TXT",
   "Title": "An Example Book",
   "ID": 1531909675
}]
```

*   **Name** is the filename.
*   **Title** is a readable name for the book, shown in game.
*   **ID** An unique ID for the book between 1 and 2147483647
*   **IsUnique** If true this book is not found inside random loots or bookshelves and must be made available directly by mods.
*   **WhenVarSet** Book is available only when a global variable is set.

This file must be placed

*   inside _StreamingAssets/Books/Mapping (__StreamingAssets/Books/Mapping/ExampleBooksPack.json)._
*   bundled inside a mod (.dfmod) inside a directory that ends with _Books/Mapping_ (for example _Assets/Game/Mods/MyMod/Books/Mapping/ExampleBooksPack.json_).

You can instantiate a book item in game with [the following method call](../api/DaggerfallWorkshop.Game.Items.ItemBuilder.yml#DaggerfallWorkshop_Game_Items_ItemBuilder_CreateBook_System_Int32_).

```
GameManager.Instance.PlayerEntity.Items.AddItem(ItemBuilder.CreateBook("ExampleBook"));
```
