# dialog

> Managing quest rumors

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
