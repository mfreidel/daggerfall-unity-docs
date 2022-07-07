# Mod Data

## Save Data

Custom dave data can be stored in a serializable class, like shown in the following example.

```
[FullSerializer.fsObject("v1")]
public class MyModSaveData
{
    public string Text;
    public List<int> Items;
}
```
A class that implements the _IHasSaveData_ interface handles the process of storing and retrieving data. The instance of this class must be assigned to the mod object (_mod.SaveDataInterface_).

This is the type of the data class.

```
public Type SaveDataType
{
    get { return typeof(MyModSaveData); }
}
```

This is called when the mod is installed for the first time.

```
public object NewSaveData()
{
    return new MyModSaveData
    {
        Text = "Default text",
        Items = new List<int>();
    };
}
```

This method asks the data to store when a new save is created (can be **null**).

```
public object GetSaveData()
{
    return new MyModSaveData
    {
        Text = text,
        Items = items;
    };
}
```

Retrieves stored data when a save is loaded.

```
public void RestoreSaveData(object saveData)
{
    var myModSaveData = (MyModSaveData)saveData;
    text = myModSaveData.Text;
    items = myModSaveData.Items;
}
```


## Custom Data

**Mod.PersistentDataDirectory** can be used to store custom data, while **Mod.TemporaryCacheDirectory** can be used to store temporary data (both of them are shared by all saves). Each mod is assigned a directory but it’s up to the mod itself to create the folder.

```
Directory.CreateDirectory(mod.PersistentDataDirectory);
string filePath = Path.Combine(mod.PersistentDataDirectory, "PersistentData.txt");
File.WriteAllText(filePath, "content");
```
