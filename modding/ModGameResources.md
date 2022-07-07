# Game Resources


## Actions

New quest actions, implementing `IQuestAction`, can be registered with `QuestMachine.__RegisterAction`, when requested by `OnRegisterCustomActions` event.


## Effects

New effects, inheriting from `BaseEntityEffect`, can be registered with `EntityEffectBroker.RegisterEffectTemplate`; It is also possible to override classic effects.


## Formulas

Formulas can be overridden from [FormulaHelper](../api/DaggerfallWorkshop.Game.Formulas.FormulaHelper.yml), registering a delegate with `FormulaHelper.RegisterOverride()`.


## Item Templates

Items data can be overridden providing a file named _ItemTemplates.json_. The property `index` is mandatory because it is the identifier for the item; everything else can be omitted not to alter classic value.

```
[
    {
        "index": 151,
        "name": "Casual Pants",
        "baseWeight": 0.5,
        "hitPoints": 150,
        "capacityOrTarget": 0,
        "basePrice": 5,
        "enchantmentPoints": 40,
        "rarity": 1,
        "variants": 4,
        "drawOrderOrEffect": 10,
        "isBluntWeapon": false,
        "isLiquid": false,
        "isOneHanded": false,
        "isIngredient": false,
        "worldTextureArchive": 204,
        "worldTextureRecord": 0,
        "playerTextureArchive": 239,
        "playerTextureRecord": 16
    }
]
```


## World and Locations

Refer to the following tutorials:

*   [Modding Tutorials: World Data, Quest Packs & New Guilds](https://forums.dfworkshop.net/viewtopic.php?f=22&t=901#p11002)
*   [Modding Tutorials: World Data Overrides](https://forums.dfworkshop.net/viewtopic.php?f=22&t=2857#p32883)
