# place

## Creating guard foes

    place aFoe at aPlace

No, not the _Halt! Halt!_ variety, unfortunately.

This action installs a quest foe or mob of foes at a quest location. Quest-related foes won't appear in a quest until this action is taken to position them within the game world. (But see `[create foe](#createfoe)`)

Guard foes loiter around the particular location, waiting for the PC to turn up, unlike the _create foe_ variety, who tend to stalk down the PC to pick a fight.


## Creating a quest item at a place

    place anItem at aPlace

This action installs a quest item at a quest location. The item generally appears as itself in the game world, rather than as a random treasure pile.

Quest-related items don't automatically appear in the game world.


## Moving NPCs around

    place anNPC at aPlace

This action installs a quest NPC at a quest location in the game world. Unlike foes and items, NPCs will appear in the game world at their birthplace when the quest begins. But the quest is free to relocate them elsewhere during the progress of the quest.
