# Quest Actions

Quest actions have appeared briefly through the examples of quest conditions, but this section documents all the known activities which can take place during a quest task.

A task may include any number of actions to respond with when its conditions for activation are met. A task may have no explicit actions at all, in which case it serves to remember that one or more conditions have been met.

Quest actions can be divided into groups for

*   Managing inventory
*   Managing geography
*   Managing characters (PC, NPCs, and foes)
*   User interactions
*   Managing special quests

But what follows is an alphabetic listing of the known quest actions.

* * *

### Adding a new questor

    add anNPC as questor

In guild quests the PC is often given some chore to perform, after which the PC must return to the questor to receive the reward or bonus that successfully completing the quest provides.

However, **X-engine** allows the NPC playing the questor role to change during the quest itself, which is one way to break the 'FedEx' monotony of questing. Different NPCs can be given responsibility for separate stages or subquests.

It is possible to adopt more than one NPC as the questor, in case your scenario is set up to have different endings.

    changeQuestor task:
        drop \_oldNPC\_ as questor
        add \_newNPC as questor

shows a quest fragment that hands control of the quest from one NPC (the \_oldNPC\_) to another (the \_newNPC\_).

* * *

### Adding an NPC's portrait

    add anNPC face
    add anNPC face saying nnnn

So-called _escort quests_ involve rescuing captured nobles, returning kidnapped children, bodyguarding officials in transit, and the like, usually display the character portrait of the NPC under escort.

When writing a quest with a suitable escort section in it, we need a way to tell **X-engine** to pin the character portrait of a quest-related person on the screen.

Optionally, the NPC can make a speech before pinning up a portrait by supplying the number of a Qrc message block.

    Message: 1011
      <ce> Oh, thank you so much for rescuing me.
      <ce> Please take me home.

    when \_trappedNPC\_ clicked
        say 1011
        remove npc \_trappedNPC\_
        add \_trappedNPC\_ face

shows an escort quest fragment where the trapped NPC makes a speech when the PC clicks on the NPC, after which the NPC image is removed from the game world and the character portrait for the NPC is overlaid on the 3D view.

It is important to remember to use the drop face command at some point during the quest after using the _add face_ so the NPC's image won't be left on the screen when the quest ends.

Note also, there are other difficulties in continuity with this action. Some permanent NPCs put up portraits that clash with their ostensible gender.

For example, pinning up the traveling portrait for Cyndassa, the maid in Castle Daggerfall, puts up the picture of a bearded, black haired man with an eye patch.

Also note, that for randomly rolled NPCs, the travel portrait is automatically associated with the dominant race of a province. So adding a face in Daggerfall province will select a portrait from the collection of Breton faces. While the same NPC in Sentinel province will select a portrait from the collection of Redguard faces.

If your quest uses the same NPC for more than one escort, (for example, you might want the PC to deliver an NPC to a place and then later on in the quest, take the NPC to a second destination) if the starting province and the destination province are different, the second pick-up of the NPC won't be guaranteed to provide the same travel portrait as the first pick-up did.

* * *

### Adding a foe's portrait

    add foe aFoe face

Some quests, notably temple quests to retrieve deranged priests, require the PC to injure a quest foe representing the priest to bring him to his senses for return to the temple. In this sort of escort quest, after injuring the foe, its character portrait is pinned on the screen until the PC returns the priest to the temple.

Those quests use this command to display the enemy's portrait on the screen. As usual, you need to drop the foe's face at some point before ending the quest.

* * *

### Managing dialog entries

    add dialog for item anItem
    add dialog for person anNPC
    add dialog for location aPlace
    add dialog for location aPlace item anItem
    add dialog for location aPlace person anNPC
    add dialog for person anNPC item anItem
    add dialog for location aPlace person anNPC item anItem

To tie chatting wih townspeople about current events to a quest, you can add stock dialog replies about quest npcs, items, and places to the replies given to the _Any news?_ query in the dialog screen. See `[dialog link](#rumore5)` for details.

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

* * *

### Magically enhancing a foe

    cast aSpell on aFoe

You can magically enhance any foe in a quest by casting one of the standard spells with this action. Of course, you can repeat the action any number of times with different spells to achieve whatever effect you would like to have.

Please see the `[_spell detection_](#spellscast)` condition for the spell enhancements which are available. For example

    cast Free\_Action spell on \_foe\_
    cast Spell\_Reflection spell on \_foe\_
    cast Shield spell on \_foe\_

strengthens \_foe\_ with the indicated enchantments.

* * *

### Changing the player's repute

    change repute with anNPC by nn

You can increase or decrease the player character's repute with any NPC associated with a quest by a Person resource.

    change repute with \_qgiver\_ by +15

would increase the PC's repute with the \_qgiver\_ and propagate that change to all the factions allied with and repudiated by \_qgiver\_.

    change repute with \_qgiver\_ by -15

would decrease the PC's repute with \_qgiver\_ and associates.

* * *

### Repeating one or more tasks

    clear aTaskName ...

You can instruct a quest to forget if a task has already been performed with the clear command. Clearing a completed task allows that task to repeat itself when its conditions are ripe. Up to four tasks/variables can be cleared with one command.

* * *

### Creating waves of attacking foes

    create aFoe every m minutes n times with k% success

You can create mobs of enemies to harry the player.

    Message: 1019
      <ce> Ha! You'll never escape with \_item\_.

    Foe \_thief\_ is 3 thief

    from 0:0 to 3:00 daily
        create \_thief\_ every 13 minutes 30 times with 75% success

    when \_thief\_ injured
        say 1019

would dispatch trios of thugs after the PC every night from midnight to 3 am. Some of the thugs won't succeed in locating the PC.

To create foes in a more guarded manner, see `[placing foes in the game world.](#placefoe)`

The spoils from the dead bodies of human foes generated by this command have the interesting properties of (a) improving in quality with each wave and (b) repeating the spoils dropped by the foes dispatched in the previous waves. The spoils are also proportional to player level, so higher level PCs tend to see better goods. But even at level 2 or 3, mithril drops aren't that uncommon.

I've had PCs garner over 5 million gold from such an episode. Even at level 2 or 3, gaining enough gold to purchase a yacht is commonplace.

The enemies spawned by _create foe_ will track the PC, both in town and inside dungeons. Some quests prefer to limit the appearance of spawned mobs to the town areas of the map. See `[_send foe_](#sendfoe)`.

Note that the `[_injured_](#attackingfoes)` condition can be triggered for each wave of attackers, while the `[_killed_](#attackingfoes)` condition will only trigger up to the number of foes determined from the Foe command, no matter how many of them are actually slaughtered by the PC.

* * *

### Creating an NPC somewhere in the game world

    create npc anNPC

This action generates anNPC (described by a Person command) at that NPC's birthplace in the current province.

    Person \_contact\_ faction The\_Cabal female anyInfo 1012

    \_foundHint\_ task:
        create npc \_contact\_

installs the \_contact\_ npc at a place in the current province, probably not local to the PC, and hopefully in a venue appropriate to the npc's group or faction.

* * *

### Creating an NPC at a specific place

    create npc at aPlace

Although I've called this _create npc_, _reserve sprite_ may be more appropriate. In the existing quests it is invariably followed by a _place npc_ at the same place, and occasionally by _place item_ at the same place or by _create foe_ at the same place.

Its purpose seems to be to inform **X-engine** that a particular site will have an item sprite or an npc sprite teleported to it during the quest.

At least, so far as my testing has shown, _create npc at_ without a corresponding _place npc_ action has no visible effect in the game world.

See the `[place NPC](#placenpc)` command for the usual way of positioning an NPC in the game world.

In general, permanent NPCs use the create npc action prior to using the place npc action. A permanent NPC used only with the place npc action isn't guaranteed to be visible within the game world.

* * *

### Curing diseases

    cure aDisease

The PC can be afflicted by disease, either during a quest (see `[make pc ill](#ill))` or through exposure to various elements (foes with poisoned blades, were-things, etc).

When a quest proceeds to the point where the PC should be cured, the _cure_ action removes the malady in question from the player character. A quest can cure any disease inflicted by the make pc ill action, as well as cure lycanthropy or vampirism. Known conditions which can be cured:

Blood\_Rot

Brain\_Fever

Caliron's\_Curse

Cholera

Chrondiasis

Consumption

Dementia

Leprosy

Plague

Red\_Death

Stomach\_Rot

Swamp\_Rot

Typhoid\_Fever

Witches'\_Pox

Wizard\_Fever

Wound\_Rot

Yellow\_Fever

Wizard\_Fever is referred to as _Wizard's bane_ or as _lunar chills_ in the quests from Bethesda.

* * *

### Change an NPC's status

    destroy anNPC

This action completely obliterates a quest _Person_ resource. This is different from `[_hide npc_](#removenpc)` in that with the latter form, the resource symbol is still available for substitution within Qrc messages blocks.

Once a quest resource has been destroyed, however, any subsequent reference to that quest resource symbol by a Qrc message will yield the infamous **BLANK** text.

This action appears in the mummy's finger quest, one of the vampire clan quests, and in Mynisera's quest to locate the courier who delivered the Emperor's letter to Aubk-i.

* * *

### Managing quest rumors

    dialog link for item anItem
    dialog link for person anNPC
    dialog link for location aSite
    dialog link for location aSite person anNPC item anItem
    dialog link for location aSite item anItem
    dialog link for person anNPC item anItem

To tie chatting wih townspeople about current events to a quest, you can add stock dialog replies about quest npcs, items, and places to the replies given to the _Any news?_ query in the dialog screen.

In addition, you can establish a link between two or more quest resources and dialog entries in the principle picklist of the dialog screen.

Note that there are two ways to incorporate quest-related information into the dialog screen: the incredibly obvious, and the not so obvious.

When you create a quest `[Item](#items), [Person](#persons), or [Place](#questplaces)` adorned with a Qrc text number, this creates an incredibly obvious dialog opportunity, by adding the thing in question to the principle pick-list under the _Tell me about_ dialog tab.

Such entries generally don't require especially good repute or etiquette on the part of the PC, although townspeople may still refuse to have anything to do with a PC they don't particularly care for.

But the presence of the entries themselves can give the PC a clue about what might possibly be going on with a quest that is currently in progress.

To exercise a PC's social skills, and thus to discriminate against PCs who lack them, a quest can silently add stock replies to the _Any News_ responses that a townsperson might say.

While the Qrc messages adorning quest items, npcs, and places are always available for reply throughout the whole time a quest is active, the stock _Any News_ replies can be added when certain quest milestones occur.

So a quest may, for example, turn on gossip about the location of a remote dungeon once the PC has made contact with a certain quest npc. Of course, the player may take no heed of the townsperson blabbering on about the goings on in some remote dungeon, but so it goes.

And the player's repute with the townsperson's affiliations seems to influence whether a quest related rumor pops up, or whether the townie just shines the player on.

These features can make quest resolution much more challenging that the typical "Fed Ex" escapades. Players often complain that quests using these features are "buggy" because their PC's social skills aren't up to the task of finishing the quest.

In addition, when you create a dialog link between two quest resources, **X-engine** will arrange to insert a dialog topic about a linked resource, but only after the PC has chatted about another part of the link first.

Suppose we say

    dialog link for person \_theStooge\_ item \_theRelic\_

during our quest startup.

In addition to the preceding discussion about rumors, when the time is ripe, the quest can expliciting insert a dialog topic for _\_theRelic\__ into the dialog picklist by using the `[add dialog](#rumor1)` command:

    add dialog for item \_theRelic\_

Now, the next time the PC uses the dialog screen, an entry for _\_theRelic\__ will be available for selection. Once the PC selects this entry and the NPC chats about _\_theRelic\__, a new entry will appear in the picklist for _\_theStooge\__, as well as _\_theRelic\__.

For a linked topic to appear in the dialog screen picklist, its symbol name must appear within the linking dialog Qrc block. That is, the Qrc text bound to \_theRelic\_ must mention \_theStooge\_ within its message block or else the linked quest resource (\_theStooge\_) will never appear as a new topic of conversation.

If the Qrc message block is subsectioned, the linkage operates independently for each subsection. So, for example, two subsections might have no mention of \_theStooge\_, but the third does. Only when the third section is displayed to the player will the linked topic be added to the dialog screen.

Or a different branch of the quest might arrange to reveal the opposite side of the link:

    add dialog for person \_theStooge\_

In this case, the next time the PC uses the dialog screen, an entry for _\_theStooge\__ will be available for selection. Once the PC selects this entry and the NPC chats about _\_theStooge\__, a new entry will appear in the picklist for _\_theRelic\__, as well as _\_theStooge\__. Of course, the Qrc message bound to \_theStooge\_ must mention \_theRelic\_ for this to occur.

Read the _cure lycanthropy_ and _cure vampirism_ quests to see how these quest elements can be used.

    dialog link for item anItem

This action adds a stock _Any News_ reply to gossip about the quest item anItem.

    dialog link for person anNPC

This action adds a stock _Any News_ reply to gossip about the quest npc anNPC.

    dialog link for location aSite

This action adds a stock _Any News_ reply to gossip about the quest location aSite.

If you have several different kinds of quest things to gossip about, you can combine location, person, and item (in that order) with one add dialog for command:

    dialog link for location \_house\_ item \_letter\_
    dialog link for person \_qgiver\_ item \_ring\_

creates one dialog bridge between _\_house\__ and _\_letter\__ and another bridge between _\_qgiver\__ and _\_ring\__.

    dialog link for for location \_store\_ person \_contact\_ item \_gem\_

creates a three-way bridge between _\_store\__, _\_contact\__, and _\_gem\__.

Note that creating a dialog link with just one quest resource in it has the property of removing that resource from the dialog pick list when the resource has an _anyInfo_ tag, _rumors_ tag, or _used_ tag.

* * *

### Dropping a questor

    drop anNPC as questor

In guild quests the PC is often given some chore to perform after which the PC must return to the questor to receive the reward or bonus that successfully completing the quest provides.

However, **X-engine** allows the NPC playing the questor role to change during the quest itself, which is one way to break the 'FedEx' monotony of questing. Different NPCs can be given responsibility for separate stages or subquests.

    changeQuestor task:
        drop \_oldNPC\_ as questor
        add \_newNPC as questor

shows a quest fragment that hands control of the quest from one NPC (the \_oldNPC\_) to another (the \_newNPC\_).

* * *

### Dropping an NPC's portrait

    drop anNPC face

So-called _escort quests_ involve rescuing captured nobles, returning kidnapped children, bodyguarding officials in transit, and the like, usually display the character portrait of the NPC under escort.

When writing a quest with a suitable escort section in it, we need a way to tell **X-engine** to remove the character portrait of a quest-related person from the screen once the escort is finished.

    Message: 1011
      <ce> Oh, thank you so much for returning with me.
      <ce> I can find my way home from here.

    when at \_npcHouse\_
        say 1011
        drop \_trappedNPC\_ face

shows an escort quest fragment where the trapped NPC makes a speech when the PC clicks on the NPC, after which the NPC image is removed from the game world and the character portrait for the NPC is overlaid on the 3D view.

It is important to remember to use the `[add face](#addface)` command at some point during the quest before using drop face so the usage counts for the NPC's image will be intact when the quest ends.

Moving the body of the NPC whose face is pinned on the screen _after_ adding its face is not advisable. If you add an npc face and then move the body of the npc, you'll have a problem when that face is finally dropped because the face won't be removed from the screen for some reason. Either move the body of the NPC before adding its face. Or hide the body of the npc and move it after dropping its face.

* * *

### Dropping a foe's portrait

    drop foe aFoe face

Some quests, notably temple quests to retrieve deranged priests, require the PC to injure a quest foe representing the priest to bring him to his senses for return to the temple. In this sort of escort quest, after injuring the foe, its character portrait is pinned on the screen until the PC returns the priest to the temple.

Those quests use this command to dispose of the enemy's portrait once the player returns to the temple to complete the quest.

* * *

### Ending the current quest

    end quest

Any task may specify that it completes the quest's activity, whether successful or not, by using the end quest command. When the _end quest_ is performed, all quest-related objects (green background) in the player's inventory are removed, other quest resources (Items, Persons, Places, and Foes) are discarded, and the player's log book emptied.

Every well-written side quest usually has an end to it. Many quests employ the common idiom of a set time to complete the quest in, and self-terminate if the job isn't completed in time.

    Clock \_timeLimit\_ 7.0:0 11.0:0

    \_timeLimit\_ task:
        end quest

specifies a time limit of 7 to 11 days for the quest. If the PC hasn't made good on the quest goal in that time, the quest ends when that clock runs down.

* * *

### Receiving items silently

    get item anItem
    get item anItem from anNPC

These actions bestow a quest-related item directly into the PC's inventory without notifying the player of the transaction. That doesn't mean a previous or subsequent action can't inform the PC about the event, just that the player isn't required to manipulate an item using the inventory screen to pick up a quest item from `(See [give item](#givepc)` to bestow an item through the inventory picklist)

_Get item from_ is similar, but specifies a particular NPC to obtain the item from. However there is no action to bestow an item on an NPC. Unless this action also checks for the presence of the NPC in the player's vicinity before bestowing the item, it behaves just the same as omitting the from clause.

(This action appears only in one quest from Bethesda, **s0000008**, if you give the totem to King Gothryd his gold reward is bestowed via this action.)

Items bestowed via get item do not lose their quest-related green backdrop in the player's inventory contents. I've never given the totem to Gothryd so I can't say how the reward is handled in that case.

\_foundHealer\_ task:
    clicked \_healer\_

\_buyAmulet\_ task:
    clicked \_healer\_ and at least 20 gold otherwise do \_tooPoor\_
    say 1013
    get item talisman
    log 1017 step 2

\_tooPoor\_ task:
    say 1015
    clear \_foundHealer\_ \_buyAmulet\_ \_tooPoor\_

This is a common idiom for buying a quest-related item from a quest NPC. If the PC approachs the quest NPC with enough cash (20 gold in this case), the PC gets a quest item after giving up the gold. If the PC is too poor, the NPC chides him and the three tasks associated with the transaction are cleared to ensure they will be executed again if the PC approaches the NPC again (presumably after getting ahold of more cash)

* * *

### Hiding items with an enemy

    give item anItem to aFoe

This action bestows a quest-related item in the inventory of a quest foe. Once bestowed, the PC can obtain the item by using his pick-pocket skill on the foe. Or the PC can obtain the item from the corpse of the foe once it is dead.

The item retains its quest-related status when the PC recovers it.

To remove a quest item after its initial bestowal, see `[take item](#moveto)`.

* * *

### Receiving items permanently

    give pc anItem

This action bestows one or more quest items through the inventory screen. Once the PC transfers an item to his inventory, that item loses its green backdrop and becomes just another piece of the player's property.

Using this action triggers the display of the Qrc _QuestComplete_ message, as though the command is always preceded by an unwritten _say QuestComplete_ command.

* * *

### Receiving a quest item with notification

    give pc anItem notify nnnn

This action bestows a quest item directly into the PC's inventory along with a speech from a Qrc message block. The item remains part of the quest with a green backdrop. Typically this action is used to deliver letters to the PC during the quest, or to start out a quest by letter.

Note that this action does not trigger the _QuestComplete_ message.

* * *

### Giving the PC nothing

    give pc nothing

Some quests appear to give the PC nothing.

Using this action can trigger the display of the _QuestComplete_ message. Possibly in conjunction with _click aQuestor_ or _drop aQuestor as questor_ conditions.

* * *

### Hiding NPCs from the player character

    hide npc anNPC

This action removes the specified quest NPC sprite from the game world. See `[restore npc](#clickable)` action to replace the NPC sprite.

Hidden NPCs are invisible to the player character, but any Qrc messages associated with that NPC will work properly; the PC is just unable to see the NPC anywhere in the game world.

* * *

### Changing the player's legal repute

    legal repute nn

This action modifies the legal standing of the PC in the current province, or the province where the quest containing this action was obtained. Positive values increase the PC's repute, while negative numbers decrease it. The change is relative to the PC's present legal standing in the province, not an absolute state.

\_failedQuest\_ task:
    legal repute -15

In this quest fragment, if the PC fails to meet the goals of the quest, his repute in the province loses 15 points.

* * *

### location aPlace number1 number2

The operation of this action is unknown. It appears in the quest startup for two quests: _b0c00y13_ (exterminate the vermin) and _a0c10y05_ (shotgun wedding).

number2 is approximately the number of game minutes allotted to the duration of the quest.

number1 seems to be a percentage, and is 100 in both examples.

This appears to serve as an \`attractor' for created foes, to direct them to the location where the PC is supposed to encounter them in the quest, rather than relying on their pathfinding skills.

* * *

### Entering messages in the log book

    log nnnn step i

This action adds a Qrc message nnnn to the player's log book as the ith entry for this quest. Quest entries number from 0. A log action which reuses an entry number i replaces the previous log entry for that step with the current one.

Since the PC may undertake several quests simultaneously, the **X-engine** uses the entry number to collect the messages of one quest in the same area of the log book.

When the quest ends, the log entries added by the quest are removed from the log book.

The number of active steps a single quest is limited to 10, so i must be between 0 and 9.

Also note, the number of concurrently active quests is limited to at most 32 quests.

Message: 1020
%qdt:
  I have accepted a quest from \_qgiver\_.

Message: 1021
%qdt:
  A friend of \_qgiver\_ has directed me to \_\_house\_.

Message: 1022
%qdt:
  I've discovered  \_qgiver\_'s \_item\_ might be hidden
  in \_\_\_dungeon\_.

--  Quest startup:
    log 1020 step 0

when clicked \_friend\_
    log 1021 step 1

when used \_letter\_
    log 1022 step 1

When this quest begins, message 1020 is added to the player's log book as the first message of this quest. Once the PC locates the quest giver's friend, a second message, 1021 is added to the log book. Later, once the PC reads a quest-related letter, this second message is replaced with message 1022.

See the `[remove log step](#changeglobal2)` action to obliterate a log book entry.

* * *

### Making quest items permanent

    make anItem permanent

This action removes the green backdrop from a quest-related item in the player's inventory, converting it into a permanent item that persists after the quest terminates.

* * *

### Infecting the player

make pc ill with aDisease

This action infects the PC with a disease. Known diseases are

Blood\_Rot

Brain\_Fever

Caliron's\_Curse

Cholera

Chrondiasis

Consumption

Dementia

Leprosy

Plague

Red\_Death

Stomach\_Rot

Swamp\_Rot

Typhoid\_Fever

Witches'\_Pox

Wizard\_Fever

Wound\_Rot

Yellow\_Fever

You _cannot_ use this action to infect the player with _lycanthropy_ or _vampirism_.

* * *

### Controlling NPC responses

    mute npc anNPC

This action causes the indicated NPC to ignore mouse clicks from the player in the game world.

* * *

### Selecting tasks randomly

    pick one of aTaskName1 aTaskName2 aTaskName3 aTaskName4

This action randomly selects one of the tasks for activation. The choice is evenly selected over the tasks, but you can skew the selection by repeating the same task.

    pick one of task1 task2 task3

gives the PC equal odds of getting _task1_, _task2_ or _task3_ while

    pick one of task1 task2 task2 task2

weights the odds 75% in favor of _task2_.

* * *

### Creating guard foes

    place aFoe at aPlace

No, not the _Halt! Halt!_ variety, unfortunately.

This action installs a quest foe or mob of foes at a quest location. Quest-related foes won't appear in a quest until this action is taken to position them within the game world. (But see `[create foe](#createfoe)`)

Guard foes loiter around the particular location, waiting for the PC to turn up, unlike the _create foe_ variety, who tend to stalk down the PC to pick a fight.

* * *

### Creating a quest item at a place

    place anItem at aPlace

This action installs a quest item at a quest location. The item generally appears as itself in the game world, rather than as a random treasure pile.

Quest-related items don't automatically appear in the game world.

* * *

### Moving NPCs around

    place anNPC at aPlace

This action installs a quest NPC at a quest location in the game world. Unlike foes and items, NPCs will appear in the game world at their birthplace when the quest begins. But the quest is free to relocate them elsewhere during the progress of the quest.

* * *

### Playing sounds

    play sound aSound every time1 minutes time2 times
    play sound aSound time1 time2

This action plays one of the 459 **Daggerfall** sound effects once every time1 game minutes while the task containing the _play sound_ command is active. Specifying time2 doesn't seem to have any effect on the behavior of the command, though.

Only the first form will accept one of the catalogued names for a sound effect, although the pure number for the effect may be used with either form.

Every model creature in Daggerfall has three separate sound effects associated with it: _distant_, _nearby_, and _in combat_. Other sound effects include various environmental conditions, like barking dogs, slamming doors, and, of course, _Halt! Halt!_

There are symbolic names for all the model creature sounds, but I haven't yet completed the environmental catalog, whose interpretation is much more subjective as to what to call the collection of assorted grunts, wheezes, and eerie noises.

Also, all the sound effects from **TES1, Arena** appear to be present. So if you've missed **Fire Daemons** or **Iron Golems**, you can at least use their respective noises.

I'll include a proper table of sound names, once the inventory is completed, but for the nonce, please see _sounds.src_ for the catalog of known sound effects.

* * *

### Playing video clips

    play video nn

This action plays a prerecorded video clip associated with the main story quest. nn indicates which of the _anim00_nn_.vid_ clips in the **arena2** directory to use.

The standard quests from Bethesda mention videos 3, 5-10, 13-15, so 0-2, 4, 11, 12 are either played automatically at certain milestones, like player death or 'bite me', or have never before been seen.

* * *

### Prompting the player

    prompt nnnn yes aTaskName1 no aTaskName2

This action prompts the player for a _yes-or-no_ response by displaying the Qrc message nnnn in a dialog box with a _yes_ and a _no_ button for the player to click.

If the player clicks the yes button, aTaskName1 is activated. On the other hand, if the player clicks the no button, aTaskName2 is activated.

* * *

### Restraining foes in the game world

    restrain foe aFoe

This action restrains the specified quest foe from attacking the PC. However, if the PC subsequently injures the foe, it will resume its attack on the PC.

* * *

### Deleting a log book entry

    remove log step nn

This action removes an entry inserted in the player's log book by a previous `[log step](#log)` action.

* * *

### Controlling NPC sprites

    restore anNPC

This action restores the indicated NPC sprite in the game world. Typically it follows a preceding `[hide npc](#removenpc)` task to make the NPC invisible, but still present so any Qrc blocks referring to the NPC still behave properly, but the PC will be unable to locate the NPC physically.

* * *

### Adding dungeons to the province

    reveal aPlace

This action reveals a remote dungeon associated with a quest on the fast travel map of the current province.

* * *

### Adding dungeons to the world map

    reveal aPlace in province aProvince# at magicNumber

This action adds a previously invisible location to the world map. aProvince# is the number for the province map on which to reveal aPlace. magicNumber is the key to the entry for aPlace in the **Map Table** of the specific province.

Magic numbers for hidden dungeon sites can be determined from the entries in arena2/maps.bsa. You can use the \`atlas' tool to create a prototype of a reveal command for any location on the fast travel map. For completeness, the method is explained below, but using the \`atlas' tool is usually easier and less error-prone.

Each province has four (4) members in maps.bsa: _mapnames.###_, _maptable.###_, _mappitem.###_, and _mapditem.###_, where _###_ is the number of a specific **Daggerfall** province.

The mapnames entry for tbe desired dungeon can be used to determine the maptable entry with the proper magic number for it in the following way:

1.  Find the offset in _mapnames_ where the desired dungeon name begins
2.  Subtract 4 from the offset and divide by 32. The remainder should be 0.
3.  Multiply the quotient by 17.
4.  This is the proper offset in maptable from which to obtain the magic number. (Offsets are numbered from 0)
5.  The five hexadecimal digits starting from offset are used to make the magic number.
6.  Write down the 5 hexadecimal digits from left to right.
7.  Reverse the byte order of these three \`virtual' bytes.
8.  This is the magic number to use with the reveal action. Preface this number with 0x to make the scenario compiler accept base 16 integers.

Here is the method applied to _Castle Necromoghan_ in _Daggerfall_ province. _Daggerfall_'s province number is 17, so we're interested in the maptable.017 and mapnames.017 members of maps.bsa.

Searching mapnames.017 for _Castle Necromoghan_ shows 0x124 for the first offset.  
((0x124 - 4) / 32) \* 17 = 0x99.  
Looking at offset 0x99 in maptable.017 shows 8f 08 2.  
Reversing the bytes we get 0x2088f or 133263 decimal. So the reveal command would be

    reveal aPlace in province 17 at 133263

### Adding an explicit rumor

    rumor mill nnnn

This action adds the Qrc message block nnnn to the set of rumors that an NPC may respond with when asked about _Any News_ in the dialog screen.

Be aware, however, that its selection is random. If you put "mission critical" information in such a Qrc block with this command, it may take the PC many chat sessions to extract the response. It would be convenient if a high etiquette skill improved the odds of getting the response out of the NPC.

* * *

### Message box

    say nnnn

This action displays a Qrc message block in the game world. If you've used message section labels to number message blocks automatically, you can use either the actual message number or the label to refer to that text.

* * *

### Creating waves of attacking foes

    send aFoe every m minutes n times with k% success

The _send foe_ command operates like the `[_create foe_](#createfoe)` command, but limits the spawning of _aFoe_ to the times when the player is above ground, generally near towns.

    Foe \_zombie\_ is Zombie

    \_doneCyndassa\_ task:
        quest 7 completed

    \_deliverLetter\_ task:
    	when \_doneWith7\_
        send \_zombie\_ every 1410 minutes -1 times with 100% success

is an excerpt from the main story where the **King of Worms** delivers his invitation to revisit **Scourg Burrow** at the start of the _Soul of the Lich_ quest.

* * *

### Starting other quests

    start quest nnnn mmmm

This action starts up one or more quests from the S000_nnnn_ family of quests. In the existing quests from Bethesda, _nnnn_ and _mmmm_ are always the same number.

* * *

### Starting a task in another quest

    start task  aTaskName

It is possible for an event in one quest to interact with an event in a seperate quest by using global tasks.

When quest **A** assigns actions to a global task number, quest **B** can request the activation of that global task in quest **A**.

Of course, for this to work out right, the authors of both quests must arrange beforehand which global task number to use to synchronize their quests. And they must also arrange to select a global task number that won't conflict with any other global tasks already in progress.

A quest that wants to make one of its tasks globally available must declare that task globally:

   variable \_synchronize\_ global 25
       log 1101 step 2
       start \_myClock\_
           :
           :

And a task that wants to activate another quest's actions must declare the same global variable then activate it at the appropriate time.

    variable \_theChase\_ global 25

    \_resumeOtherQuest\_ task:
      clicked \_myNpc\_
      start task \_theChase\_

which would then start the \_synchronize\_ step in the other quest.

Note that there may be more than one other quest listening to the same global variable.

The main quest uses 0-4, 6-9, 11-14, 16-18, 21, 23, 25, 28-29, 31, 33, 36, and 42-43 for global task numbers.

The following table documents the function of the global states used by the main quest.

LiftedCurse

0

GothrydGotTotem

1

KingOfWormsGotTotem

2

GortwogGotTotem

3

AkorithiGotTotem

4

TookTheCure

5\[1\]

UnderkingGotTotem

6

EadwyreGotTotem

7

BrisiennaGotTotem

8

MedoraGotHorn

9

GothrydEnding

11

OpenedShapeshifters

10\[1\]

KingOfWormsEnding

12

GortwogEnding

13

AkorithiEnding

14

UnderkingEnding

16

EadwyreEnding

17

BrisiennaEnding

18

UnknownElysanna

21

MorgiahSatisfied

23

ElysannaSatisfied

25

BarenziahSatisfied

28

MyniseraSatisfied

29

MetLadyBrisienna

31

KingOfWormsSatisfied

33

FinishedMantellanCrux

36

UnknownHelseth

42

LysandusSatisfied

43

\[1\]Used by **Werefall**

Presumably the residual numbers 5, 10, 15, 19, 20, 22, 24, 26, 27, 30, 32, 34, 35, 37-41 can be used by any quests you choose to create.

There are, in fact, 64 global variables. Their current values are stored in consecutive bytes in _savevars.dat_, beginning at offset 0x34f (0-based).

* * *

### Starting a clock

    start timer aClock

This action (re)starts the specified timer.

* * *

### Stopping a clock

    stop timer aClock

This action stops the specified timer, without triggering the actions scheduled by its alarm task.

* * *

### Removing quest items

    take anItem from pc

This action removes a previously created quest item `(see the [get/give](#getitem) series of actions or [place anItem](#placeitem)` to create a quest-related item) from the player's inventory, and from the game world.

* * *

### Teleporting the PC

    teleport pc to aPlace
    transfer pc inside aPlace  province#  magicNumber

You can relocate the PC to another position in the current province by using the teleport pc form of the command. Existing quests use this to relocate the PC to a remote dungeon trap in the same province, as though the teleport spell anchor were placed there.

It isn't known whether this form of the command will teleport the PC to permanent sites.

The alternate form, transfer pc, is used at the end of the main story when Nulfaga teleports the PC to the final dungeon of the Mantellan Crux. It is also used to return the PC to Nulfaga when the Crux quest is completed.

province# and magicNumber are determined as for the `[reveal command.](#revealglobal)`
