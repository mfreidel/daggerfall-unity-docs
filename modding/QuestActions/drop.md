# drop

## Dropping a questor

```
drop anNPC as questor
```

In guild quests the PC is often given some chore to perform after which the PC must return to the questor to receive the reward or bonus that successfully completing the quest provides.

However, **X-engine** allows the NPC playing the questor role to change during the quest itself, which is one way to break the 'FedEx' monotony of questing. Different NPCs can be given responsibility for separate stages or subquests.

```
changeQuestor task:
    drop _oldNPC_ as questor
    add _newNPC as questor
```

shows a quest fragment that hands control of the quest from one NPC (the `_oldNPC_`) to another (the `_newNPC_`).


## Dropping an NPC's portrait

```
drop anNPC face
```

So-called _escort quests_ involve rescuing captured nobles, returning kidnapped children, bodyguarding officials in transit, and the like, usually display the character portrait of the NPC under escort.

When writing a quest with a suitable escort section in it, we need a way to tell **X-engine** to remove the character portrait of a quest-related person from the screen once the escort is finished.

```
Message: 1011
  <ce> Oh, thank you so much for returning with me.
  <ce> I can find my way home from here.

when at _npcHouse_
    say 1011
    drop _trappedNPC_ face
```

shows an escort quest fragment where the trapped NPC makes a speech when the PC clicks on the NPC, after which the NPC image is removed from the game world and the character portrait for the NPC is overlaid on the 3D view.

It is important to remember to use the [add face](./add.md#adding-an-npcs-portrait) command at some point during the quest before using drop face so the usage counts for the NPC's image will be intact when the quest ends.

Moving the body of the NPC whose face is pinned on the screen _after_ adding its face is not advisable. If you add an npc face and then move the body of the npc, you'll have a problem when that face is finally dropped because the face won't be removed from the screen for some reason. Either move the body of the NPC before adding its face. Or hide the body of the npc and move it after dropping its face.


## Dropping a foe's portrait

```
drop foe aFoe face
```

Some quests, notably temple quests to retrieve deranged priests, require the PC to injure a quest foe representing the priest to bring him to his senses for return to the temple. In this sort of escort quest, after injuring the foe, its character portrait is pinned on the screen until the PC returns the priest to the temple.

Those quests use this command to dispose of the enemy's portrait once the player returns to the temple to complete the quest.
