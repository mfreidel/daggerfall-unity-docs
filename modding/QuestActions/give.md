# give

## Hiding items with an enemy

    give item anItem to aFoe

This action bestows a quest-related item in the inventory of a quest foe. Once bestowed, the PC can obtain the item by using his pick-pocket skill on the foe. Or the PC can obtain the item from the corpse of the foe once it is dead.

The item retains its quest-related status when the PC recovers it.

To remove a quest item after its initial bestowal, see `[take item](#moveto)`.


## Receiving items permanently

    give pc anItem

This action bestows one or more quest items through the inventory screen. Once the PC transfers an item to his inventory, that item loses its green backdrop and becomes just another piece of the player's property.

Using this action triggers the display of the Qrc _QuestComplete_ message, as though the command is always preceded by an unwritten _say QuestComplete_ command.


## Receiving a quest item with notification

    give pc anItem notify nnnn

This action bestows a quest item directly into the PC's inventory along with a speech from a Qrc message block. The item remains part of the quest with a green backdrop. Typically this action is used to deliver letters to the PC during the quest, or to start out a quest by letter.

Note that this action does not trigger the _QuestComplete_ message.


## Giving the PC nothing

    give pc nothing

Some quests appear to give the PC nothing.

Using this action can trigger the display of the _QuestComplete_ message. Possibly in conjunction with _click aQuestor_ or _drop aQuestor as questor_ conditions.
