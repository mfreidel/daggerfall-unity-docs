# restore

> Controlling NPC sprites

    restore anNPC

This action restores the indicated NPC sprite in the game world. Typically it follows a preceding `[hide npc](#removenpc)` task to make the NPC invisible, but still present so any Qrc blocks referring to the NPC still behave properly, but the PC will be unable to locate the NPC physically.
