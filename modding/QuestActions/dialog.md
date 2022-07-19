# dialog

> Managing quest rumors

```
dialog link for item anItem
dialog link for person anNPC
dialog link for location aSite
dialog link for location aSite person anNPC item anItem
dialog link for location aSite item anItem
dialog link for person anNPC item anItem
```

To tie chatting with townspeople about current events to a quest, you can add stock dialog replies about quest NPCs, items, and places to the replies given to the _Any news?_ query in the dialog screen.

In addition, you can establish a link between two or more quest resources and dialog entries in the principle picklist of the dialog screen.


## Overview

There are two ways to incorporate quest-related information into the dialog screen: the _Tell me about_ dialog tab via the `add dialog` command, and the _Any News_ rumors via the `dialog link` command.


### Tell me about dialog

When you create a quest [Item](../QuestOrganization.md#quest-items), [Person](../QuestOrganization.md#quest-persons), or [Place](../QuestOrganization.md#quest-places) adorned with a Qrc text number, this creates an incredibly obvious dialog opportunity, by adding the thing in question to the principle pick-list under the _Tell me about_ dialog tab.

Such entries generally don't require especially good repute or etiquette on the part of the PC, although townspeople may still refuse to have anything to do with a PC they don't particularly care for. The presence of the entries themselves can give the PC a clue about what might possibly be going on with a quest that is currently in progress.


### Any News rumors

> [!NOTE]
> These features can make quest resolution much more challenging than the typical "Fed Ex" escapades. Players often complain that quests using these features are "buggy" because their PC's social skills aren't up to the task of finishing the quest.

To exercise a PC's social skills, and thus to discriminate against PCs who lack them, a quest can silently add stock replies to the _Any News_ responses that a townsperson might say.

While the Qrc messages adorning quest items, NPCs, and places are always available for reply throughout the whole time a quest is active, the stock _Any News_ replies can be added when certain quest milestones occur.

So a quest may, for example, turn on gossip about the location of a remote dungeon once the PC has made contact with a certain quest npc. Of course, the player may take no heed of the townsperson blabbering on about the goings on in some remote dungeon, but so it goes.

And the player's repute with the townsperson's affiliations seems to influence whether a quest related rumor pops up, or whether the townie just shines the player on.

In addition, when you create a dialog link between two quest resources, **X-engine** will arrange to insert a dialog topic about a linked resource, but only after the PC has chatted about another part of the link first.


## Usage Details

### Starting with a rumor

Suppose we say the following during our quest startup:

```
dialog link for person _theStooge_ item _theRelic_
```

In addition to the preceding discussion about rumors, when the time is ripe, the quest can explicitly insert a dialog topic for `_theRelic_` into the dialog picklist by using the [add dialog](./add.md#managing-dialog-entries) command:

```
add dialog for item _theRelic_
```

Now, the next time the PC uses the dialog screen, an entry for `_theRelic_` will be available for selection. Once the PC selects this entry and the NPC chats about `_theRelic_`, a new entry will appear in the picklist for `_theStooge_`, as well as `_theRelic_`.

> [!IMPORTANT]
> For a linked topic to appear in the dialog screen picklist, its symbol name must appear within the linking dialog Qrc block. That is, the Qrc text bound to `_theRelic_` must mention `_theStooge_` within its message block or else the linked quest resource (`_theStooge_`) will never appear as a new topic of conversation.


### Qrc message with subsections

If the Qrc message block is subsectioned, the linkage operates independently for each subsection. So, for example, two subsections might have no mention of `_theStooge_`, but the third does. Only when the third section is displayed to the player will the linked topic be added to the dialog screen.

Or a different branch of the quest might arrange to reveal the opposite side of the link:

```
add dialog for person _theStooge_
```

In this case, the next time the PC uses the dialog screen, an entry for `_theStooge_` will be available for selection. Once the PC selects this entry and the NPC chats about `_theStooge_`, a new entry will appear in the picklist for `_theRelic_`, as well as `_theStooge_`. Of course, the Qrc message bound to `_theStooge_` must mention `_theRelic_` for this to occur.

> [!TIP]
> Decompile and read the _cure lycanthropy_ and _cure vampirism_ quests to see how these quest elements can be used.


### Multiple sources of gossip

```
dialog link for item anItem
```

This action adds a stock _Any News_ reply to gossip about the quest item `anItem`.

```
dialog link for person anNPC
```

This action adds a stock _Any News_ reply to gossip about the quest NPC `anNPC`.


```
dialog link for location aSite
```

This action adds a stock _Any News_ reply to gossip about the quest location `aSite`.

If you have several different kinds of quest things to gossip about, you can combine location, person, and item (in that order) with one add dialog for command:

```
dialog link for location _house_ item _letter_
dialog link for person _qgiver_ item _ring_
```

creates one dialog bridge between `_house_` and `_letter_` and another bridge between `_qgiver_` and `_ring_`.

```
dialog link for for location _store_ person _contact_ item _gem_
```

creates a three-way bridge between `_store_`, `_contact_`, and `_gem_`.

> [!NOTE]
> Creating a dialog link with just one quest resource in it has the property of removing that resource from the dialog pick list when the resource has an `_anyInfo_` tag, `_rumors_` tag, or `_used_` tag.
