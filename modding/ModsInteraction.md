# Mods Interaction

The **ModManager** singleton allows to retreve a **Mod** instance from its title.

```
Mod mod = ModManager.Instance.GetMod("modTitle");
bool modFound = mod != null;
bool modStarted = mod != null && mod.IsReady;
```

> Consider using **GetModFromGUID(string modGUID)** to avoid breaking if the title is changed.


## Message Receiver

The message receiver allows to exchange messages and data with other mods. The receiving mod must have a delegate of type **DFModMessageReceiver** assigned to **Mod.MessageReceiver**. A reply can be sent with the **callback** parameter.

```
void ModManager.Instance.SendModMessage(string modTitle, string message, object data = null, DFModMessageCallback callback = null);
void DFModMessageReceiver(string message, object data, DFModMessageCallback callBack);
void DFModMessageCallback(string message, object data);
```

A simple example:

```
mod.MessageReceiver = (string message, object data, DFModMessageCallback callBack) =>
{
    if (message == "numberRequest" && callBack != null)
        callBack("numberReply", 0);
};
```

```
ModManager.Instance.SendModMessage("modTitle", "numberRequest", null, (string message, object data) =>
{
    int number = (int)data;
});
```

The mod message system can also be used to handle events. Note that the receiver is setup from Awake (or the static entry point if you prefer) while the handler registration is performed from Start. This is consistent with the pattern of using Awake to initialize self and Start to access other objects, ensuring that you aren’t accessing something that doesn’t exist yet.

```
public event Action OnGameObjectSpawned;

void Awake()
{
    mod.MessageReceiver = (string message, object data, DFModMessageCallback callback) =>
    {
        switch (message)
        {
            case "RegisterOnGameObjectSpawned":
                OnGameObjectSpawned += data as Action<GameObject>;
                break;
        }
    };
}
```

```
void Start()
{
    ModManager.Instance.SendModMessage("Mod Title", "RegisterOnGameObjectSpawned", (Action<GameObject>)((go) =>
    {
        Debug.Log(go.name);
    }));
}
```


## Dependencies

It is possible for a mod to declare other mods as dependencies that must also be installed, or define criteria to validate local load order. For example you can enforce that your mod is positioned below another one if is available or state compatibility only with a certain version.

To do so, open the mod manifest file (.dfmod.json) and add a new property named _Dependencies:_

```
"Files": [
    ],
"Dependencies": [
    {
        "Name": "example-mod",
        "IsOptional": false,
        "IsPeer": false,
        "Version": "1.0.0"
    }
]
```

These are the supported properties:

*   **Name**: Name of target mod.
*   **IsOptional**: If true, target mod doesn’t need to be available, but must validate these criteria if it is.
*   **IsPeer**: If true, target mod can be positioned anywhere in the load order, otherwise must be positioned above.
*   **Version**: If not null this string is the minimum accepted version with format X.Y.Z. Pre-release identifiers following an hyphen are ignored in target version so they must be omitted here. For example “1.0.0” is equal to “1.0.0-rc.1”.
