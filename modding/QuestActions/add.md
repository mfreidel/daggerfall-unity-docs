# add

## Adding a new questor

```
add anNPC as questor
```

In guild quests the PC is often given some chore to perform, after which the PC must return to the questor to receive the reward or bonus that successfully completing the quest provides.

However, **X-engine** allows the NPC playing the questor role to change during the quest itself, which is one way to break the 'FedEx' monotony of questing. Different NPCs can be given responsibility for separate stages or subquests.

It is possible to adopt more than one NPC as the questor, in case your scenario is set up to have different endings.

```
    changeQuestor task:
        drop _oldNPC_ as questor
        add _newNPC as questor
```

shows a quest fragment that hands control of the quest from one NPC (the \_oldNPC\_) to another (the \_newNPC\_).


## Adding an NPC's portrait

```
    add anNPC face
    add anNPC face saying nnnn
```

So-called _escort quests_ involve rescuing captured nobles, returning kidnapped children, bodyguarding officials in transit, and the like, usually display the character portrait of the NPC under escort.

When writing a quest with a suitable escort section in it, we need a way to tell **X-engine** to pin the character portrait of a quest-related person on the screen.

Optionally, the NPC can make a speech before pinning up a portrait by supplying the number of a Qrc message block.

```
    Message: 1011
      <ce> Oh, thank you so much for rescuing me.
      <ce> Please take me home.

    when _trappedNPC_ clicked
        say 1011
        remove npc _trappedNPC_
        add _trappedNPC_ face
```

shows an escort quest fragment where the trapped NPC makes a speech when the PC clicks on the NPC, after which the NPC image is removed from the game world and the character portrait for the NPC is overlaid on the 3D view.

It is important to remember to use the drop face command at some point during the quest after using the _add face_ so the NPC's image won't be left on the screen when the quest ends.

Note also, there are other difficulties in continuity with this action. Some permanent NPCs put up portraits that clash with their ostensible gender.

For example, pinning up the traveling portrait for Cyndassa, the maid in Castle Daggerfall, puts up the picture of a bearded, black haired man with an eye patch.

Also note, that for randomly rolled NPCs, the travel portrait is automatically associated with the dominant race of a province. So adding a face in Daggerfall province will select a portrait from the collection of Breton faces. While the same NPC in Sentinel province will select a portrait from the collection of Redguard faces.

If your quest uses the same NPC for more than one escort, (for example, you might want the PC to deliver an NPC to a place and then later on in the quest, take the NPC to a second destination) if the starting province and the destination province are different, the second pick-up of the NPC won't be guaranteed to provide the same travel portrait as the first pick-up did.


## Adding a foe's portrait

```
    add foe aFoe face
```

Some quests, notably temple quests to retrieve deranged priests, require the PC to injure a quest foe representing the priest to bring him to his senses for return to the temple. In this sort of escort quest, after injuring the foe, its character portrait is pinned on the screen until the PC returns the priest to the temple.

Those quests use this command to display the enemy's portrait on the screen. As usual, you need to drop the foe's face at some point before ending the quest.

## Managing dialog entries

```
    add dialog for item anItem
    add dialog for person anNPC
    add dialog for location aPlace
    add dialog for location aPlace item anItem
    add dialog for location aPlace person anNPC
    add dialog for person anNPC item anItem
    add dialog for location aPlace person anNPC item anItem
```

To tie chatting with townspeople about current events to a quest, you can add stock dialog replies about quest NPCs, items, and places to the replies given to the _Any news?_ query in the dialog screen. See [Managing quest rumors](#Managing-quest-rumors) for details.

You can also add additional entries to the picklist under the _Tell me about_ dialog tab. Note that there are two ways to incorporate quest-related information into the dialog screen: the incredibly obvious, and the not so obvious.

When you create a quest `[Item](#items), [Person](#persons), or [Place](#questplaces)` adorned with a Qrc text number, this creates an incredibly obvious dialog opportunity, by adding the thing in question to the principle pick-list under the _Tell me about_ dialog tab.

Such entries generally don't require especially good repute or etiquette on the part of the PC, although townspeople may still refuse to have anything to do with a PC they don't particularly care for.

But the presence of the entries themselves can give the PC a clue about what might possibly be going on with a quest that is currently in progress.

To exercise a PC's social skills, and thus to discriminate against PCs who lack them, a quest can silently add stock replies to the _Any News_ responses that a townsperson might say, as well as create links between quest resources which are logically related to each for the purpose of the quest.

While the Qrc messages adorning quest items, npcs, and places are always available for reply throughout the whole time a quest is active, the _Tell me about_ dialog entries can be extended when certain quest milestones occur.

So a quest may, for example, turn on gossip about the location of a remote dungeon once the PC has made contact with a certain quest npc. Of course, the player may take no heed of the townsperson blabbering on about the goings on in some remote dungeon, but so it goes.

These features can make quest resolution much more challenging that the typical "Fed Ex" escapades. Players often complain that quests using these features are "buggy" because their PC's social skills aren't up to the task of finishing the quest.

You can add stock dialog replies for any combination of a quest item, npc, or place with one add dialog for command. And you can repeat an add dialog for command as many times as needed to add replies for as many quest things as you need to.

Read the _cure lycanthropy_ and _cure vampirism_ quests to see how these quest elements can be used.

    add dialog for item anItem

This action adds _anItem_ to the _Tell me about_ dialog picklist and includes the Qrc text block from the item's _anyInfo_ tag. If the item has no _anyInfo_ tag, no entry for the item will be created.

When the player selects that item from the dialog picklist, the NPC may recite some useful information about the item, depending on how well-respected the PC is with respect to the NPC's affiliations.

If the item has been linked to another quest resource by the `[_dialog link_](#rumor5)` command, when the NPC recites the Qrc message from the item's _anyInfo_ tag, the linked quest resource name is added to the dialog pick list, just as though the quest scenario issued an appropriate _add dialog_ command for the linked resource.

Please note, if you want to use linked dialog entries in a quest, you must first use the `[_dialog link_](#rumor5)` command to establish the desired relationship before using the _add dialog_ command to update the dialog picklist.

    add dialog for person anNPC

This action adds _anNPC_ to the _Tell me about_ dialog picklist and includes the Qrc text block from the person's _anyInfo_ tag. If the person has no _anyInfo_ tag, no entry for the person will be created.

If the person has been linked to another quest resource by the `[_dialog link_](#rumor5)` command, when the NPC recites the Qrc message from the person's _anyInfo_ tag, the linked quest resource name is added to the dialog pick list, just as though the quest scenario issued an appropriate _add dialog_ command for the linked resource.

Please note, if you want use linked dialog entries in a quest, you must first use the `[_dialog link_](#rumor5)` command to establish the desired relationship before using the _add dialog_ command to update the dialog picklist.

    add dialog for location aSite

This action adds _aSite_ to the _Tell me about_ dialog picklist and includes the Qrc text block from the place's _anyInfo_ tag. If the place has no _anyInfo_ tag, no entry for the place will be created.

If the location has been linked to another quest resource by the `[_dialog link_](#rumor5)` command, when the NPC recites the Qrc message from the place's _anyInfo_ tag, the linked quest resource name is added to the dialog pick list, just as though the quest scenario issued an appropriate _add dialog_ command for the linked resource.

Please note, if you want use linked dialog entries in a quest, you must first use the `[_dialog link_](#rumor5)` command to establish the desired relationship before using the _add dialog_ command to update the dialog picklist.

If you have several different kinds of quest resources to gossip about, you can combine location, person, and item (in that order) with one add dialog for command:

    add dialog for location \_store\_ person \_contact\_ item \_gem\_
    add dialog for location \_house\_ item \_letter\_
    add dialog for person \_qgiver\_ item \_ring\_
