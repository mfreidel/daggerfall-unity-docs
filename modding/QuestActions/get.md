# get

> Receiving items silently

```
get item anItem
get item anItem from anNPC
```

> [!NOTE]
> Items bestowed via `get item` do not lose their quest-related green backdrop in the player's inventory contents. How this is handled by the game has not been fully investigated or documented.

These actions bestow a quest-related item directly into the PC's inventory without notifying the player of the transaction. That doesn't mean a previous or subsequent action can't inform the PC about the event, just that the player isn't required to manipulate an item using the inventory screen to pick up a quest item from (See [give item](./give.md) to bestow an item through the inventory picklist)

_Get item from_ is similar, but specifies a particular NPC to obtain the item from. However there is no action to bestow an item on an NPC. Unless this action also checks for the presence of the NPC in the player's vicinity before bestowing the item, it behaves just the same as omitting the from clause.

```
_foundHealer_ task:
    clicked _healer_

_buyAmulet_ task:
    clicked _healer_ and at least 20 gold otherwise do _tooPoor_
    say 1013
    get item talisman
    log 1017 step 2

_tooPoor_ task:
    say 1015
    clear _foundHealer_ _buyAmulet_ _tooPoor
```

This is a common idiom for buying a quest-related item from a quest NPC. If the PC approaches the quest NPC with enough cash (20 gold in this case), the PC gets a quest item after giving up the gold. If the PC is too poor, the NPC chides him and the three tasks associated with the transaction are cleared to ensure they will be executed again if the PC approaches the NPC again (presumably after getting ahold of more cash)

> [!TIP]
> This action appears in only one quest from Bethesda: **s0000008**. If you give the totem to King Gothryd, then his gold reward is bestowed via this action.
