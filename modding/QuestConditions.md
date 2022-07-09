# Quest Condition/Responses

The quest condition/response section is probably the most involved section of the quest. Here is the logic or scripting of what happens during the quest.

This section can be thought of as tasks or steps which are triggered by various events in the game world. It probably helps if you've been exposed to some programming concepts before, but it isn't essential.

Each task of the quest has a symbol name associated with it, which can be used in other tasks to determine whether that step has been taken yet.

The general pattern of a task is:

    \_symbol\_  task:
        action1
        action2
          :
          :

         or

    \_symbol\_  task:
    	condition1
    	condition2
    	  :
        action1
        action2
          :
          :

          or

    \_symbol\_  task:
    	condition1
    	condition2
    	  :
          :

          or

    until  \_symbol\_  performed:
    	action1
    	action2
    	  :
          :

where an action is the desired quest activity and condition is the quest event to trigger the desired action.

Quest tasks are usually written in what's called their positive-definite form, and so are usually performed but once. However the _until performed_ form creates a task to perform over and over again until another task is finished.

There are also task forms which condense the task and conditions together as a sort of shorthand.

For example, suppose part of a quest involves locating an NPC to receive a hint or clue. When the player locates that person, the quest must tell the **X-engine** what to do. We can create a task to handle this event in the game world.

    \_npcClicked\_ task:
        clicked \_npc\_
        say 1014
        log 1015 step 1
        reveal \_dungeon\_
        place \_item\_ at \_dungeon\_

which says roughly when the player clicks on \_npc\_ the NPC makes a speech from a Qrc message block, adds an entry to the player's log book, adds a dungeon to the province map, and hides a quest item in that dungeon.

Life isn't quite that simple though. What happens if the PC returns and clicks the NPC again? Does he get a free item? Oops. A better solution might be:

    -- This task remembers whether the PC has clicked \_npc\_
    \_npcClicked\_ when \_npc\_ clicked

    -- This  task remembers whether we've hidden the quest item or not.
    \_itemHidden\_ when \_hideItem\_

    -- This task checks whether the PC has clicked \_npc\_ and only
    -- proceeds if we've not yet hidden the quest item.
    \_hideItem\_ task:
    when \_npcClicked\_ and not \_itemHidden\_
        say 1014
        log 1015 step 1
        reveal \_dungeon\_
        place \_item\_ at \_dungeon\_

    -- This task intervenes if the PC clicks \_npc\_ again
    -- after we've hidden the quest item in the dungeon.
    \_alreadyHidden\_ task:
    when \_npcClicked\_ and \_itemHidden\_
        say 1016

If the PC tries to 'cheat' by clicking the NPC again, the NPC chides the PC with message 1016.

* * *

Quest conditions
----------------

Apart from the quest start-up actions and clock time-outs, quest tasks are triggered by conditions or events within the game world which the quest wants to take special notice of, usually to supply some stimulus to the player character which would not otherwise arise.

This section catalogs the game world events which a quest can check for, to take appropriate action on as the circumstances require.

The game world events that a quest may capture are

*   cast aSpell spell do aTask
*   clicked anItem
*   clicked anNPC
*   clicked anNPC and at least anAmount gold otherwise do aTask
*   daily from aTime1 to aTime2
*   dropped anItem at aPlace
*   faction aFaction available
*   from aTime1 to aTime2 daily
*   have anItem set aVariable
*   injured aFoe
*   killed aFoe
*   level nnnn completed
*   pc at aPlace do aTask
*   repute with anNPC exceeds nn do aTask
*   toting anItem and anNPC clicked
*   anItem used do aTask
*   anItem used saying nnnn do aTask
*   when aFoe killed
*   when aTaskHasBeenPerformed
*   when anItem clicked
*   when anItem used
*   when anItem used saying nnnn
*   when anNPC clicked
*   when anItem dropped at aPlace
*   when aSpell spell cast
*   when repute with aFaction is at least nnn
*   when pc at aPlace
*   when pc not at aPlace
*   when aSpell cast
*   when toting anItem and anNPC clicked

Some of the conditions seem to describe the same thing (e.g., _killed aFoe_ vs _when aFoe killed_) and they do refer to the same game world event, but have different contexts within the quest source file.

For the **X-engine**, conditions always have task names associated with them. But as a quest author you don't necessarily care about the task name a condition is associated with unless you need to refer to the completion of that task elsewhere in the quest.

So many of the conditions have two equivalent forms of expression, one which must follow a named task, and one which does not. The basic rule is that the forms beginning _when ..._ create their own task names automatically while the other forms do not, and so must appear within a task named by the _task:_ directive.

The detailed descriptions of the quest conditions follows.

* * *

### Locating things in the game world

    clicked aThing
    when aThing clicked
    toting anItem and anNPC clicked

When the player character clicks on a quest related item or NPC this condition will schedule the actions associated with this task for execution by the **X-engine**. aThing must be a quest item created by an Item command, or a quest NPC created by a Person command.

    Message: 1017
       <ce> Your quest is complete.

    when \_item1\_ clicked
        say 1017
        end quest  

In this quest fragment, when the PC clicks the mouse on \_item1\_ in the game world, message number 1017 is displayed and the quest is finished.

Alternatively, the same task could be written:

    foundItem task:
        clicked \_item1\_
        say 1017
        end quest

Sometimes you only want to take action if the PC approachs your NPC with a specific quest item

    Message: 1020
      <ce> Thank you for returning my widget.

    returnItem task:
        toting \_item1\_ and \_questor\_ clicked
        say 1020

So the questor won't congradulate you unless you return with the quest item he's waiting for.

This last form, _toting and clicked_ has an important side-effect when you include it in a scenario: **_The item is destroyed when both parts of the condition are true_**.

* * *

### By the player's cash on hand

    clicked anNPC and at least anAmount gold otherwise do aTask

When the PC clicks on a quest NPC in order to obtain a quest item from the NPC, the quest can assign a cost to the quest item and take different actions if the PC doesn't have the requisite cash available.

If the PC has at least the specified amount of gold on hand, that number of gold pieces will be taken from the PC and the remainder of the actions in this task will be performed.

If the PC doesn't have enough gold (and it must be available as pieces of gold; letters of credit don't seem to apply) none of the actions in the task are performed. Instead, the **X-engine** schedules the other task mentioned by the condition.

    \_buyTalisman\_ task:
        clicked \_healer\_ and at least 20 gold otherwise do \_pcTooPoor\_
        say 1013
        get item talisman
        log 1017 step 2

    \_pcTooPoor\_ task:
        say 1015
        clear \_buyTalisman\_ \_pcTooPoor\_

* * *

### By the clock

    daily from aTime1 to aTime2
    from aTime1 to aTime2 daily

A quest may direct the **X-engine** to perform some task on a daily schedule. Often this involves sending nasties to plague the player, but any quest action is possible. This condition is used in the main story for example to make the ghost of King Lysandus haunt Daggerfall City for several hours after sunset. For example

    from 06:00 to 10:00 daily
        create \_foe\_ every 10 minutes 7 times with 100% success

or

    plague task:
        daily from 6:00 to 10:00
        create \_foe\_ every 10 minutes 7 times with 100% success

would create waves of nasties to plague the player character every morning from 6 to 10 for the duration of the quest.

Note also that each Clock command creates an implicit time-out condition. Create a task with the same name as the clock to activate when the clock runs out. Note that a Clock resource doesn't automatically start ticking, and must be started by using the `[_start timer_](#starttimer)` action.

    Message: 1019
      <ce> Well, you've run out of time now.

    Clock \_tooSlow\_ 12.0

    -- Quest startup:
        start timer \_tooSlow\_

    \_tooSlow\_ task:
        say 1019
        end quest

In this quest fragment, if the PC is still plodding along after 12 days on this quest, chide him and terminate the quest.

* * *

### Checking the PC's repute

A quest can check the player's repute with a particular quest NPC and take different actions according to whether the NPC likes or dislikes the PC.

	repute with anNPC exceeds nn do aTask

is the condition to schedule a different task if the NPC favors the player character enough. NN is the PC's minimum repute with the NPC, usually between 0 and 100, although the PC's reputation can be negative.

    Message: 1019
      <ce> I suppose this will have to do.
      <ce> Couldn't you have brought a fresher sample?

    Message: 1020
      <ce> I say, that was quite good work for a %ra.
      <ce> Here, take this extra reward for special service.

    checkForBonus task:
        repute with \_npc\_ exceeds 41 do bonus

    variable bonus

    bonusReward task:
        when deadFoe and bonus
        say 1020
        give pc \_reward\_ \_extraReward\_
        end quest

    regularReward task:
        when deadFoe and not bonus
        say 1019
        give pc \_reward\_
        end quest

which bestows one reward if the PC's reputation with the NPC is smaller than 42 and bestows two rewards if the reputation is larger than 41. You can write quests to shine the PC on if the NPC doesn't like the PC's previous affilliations.

* * *

### Using items

    have anItem set aVariable
    have anItem do aTask
    anItem used do aTask
    anItem used saying nnnn do aTask

There are times when a quest author wants to take some special action when the player manipulates an object pertaining to the quest.

A quest can detect when the player has a particular item in the inventory by using the _have anItem_ condition. The two forms are really equivalent, for in the **X-engine** a variable is just a task without any conditions or actions.

    Message: 1015
      <ce> You notice a sudden change in your surroundings.

    ambush task:
        say 1015
        move \_foe\_ to pc

    waiting task:
        have \_questItem\_ do ambush

would set up to ambush the PC after a quest item was added to the inventory.

    Message: 1015
      <ce> As you examine the parchment
      <ce> you notice your surroundings change.

    ambush task:
        say 1015
        teleport pc to \_newDungeon\_

    waiting task:
        \_questItem\_ used do ambush

would teleport the PC to a different dungeon when the quest item was used in the inventory screen (to read a quest letter perhaps)

* * *

### Attacking foes

A quest can detect when the PC first injures an enemy in a fight or when the enemy is dead.

    injured aFoe
    killed aFoe
    killed nn aFoe
    when aFoe injured
    when aFoe killed

For example

    Message: 1099
      <ce> No, I won't let you kill me.  Begone!

    When \_foe\_ injured
        say 1099
        teleport pc to \_newDungeon\_

which contrives to teleport the player elsewhere if the specified foe has been injured.

    Message: 1099
      <ce> Congradulations, you've killed the orc commander.

    When \_foe\_ killed
        say 1099
        end quest

which pops up a message box when the PC slaughters the specified foe.

Recall that the Foe command will create mobs of kindred foes. You can specify how many of the mob must be slain for the condition

    Foe \_foe\_ is 17 vampire

    enoughDead task:
        killed 5 \_foe\_
        get item \_reward\_
        teleport pc to \_newDungeon\_

would trigger the requested actions after killing 5 of the 17 vampires.

Note that the _killed_ task will be permanently retired by the **X-engine** once the number of foes created by the associated _Foe_ command have been slaughtered.

That sounds reasonable enough, but notice that it clashs with the use of the _create foe_ command, which may create more foes to be killed than can be tested by the _killed_ condition.

Even worse, you can't use several kill commands to check for different numbers of foes slaughtered. You can have only one _killed_ action for each _Foe_ declaration. If you provide more than one, the action response becomes _unpredictable_.

* * *

### Locating the PC in the game world

Sometimes you want to take some action when the PC arrives at a particular quest location.

    pc at aPlace do aTask
    when pc at aPlace
    when pc not at aPlace
    pc at anNPC do aTask
    when pc at anNPC
    when pc not at anNPC

So for example,

    when pc at \_dungeon\_
        place \_questItem\_ at \_dungeon\_
        place \_foe\_ at \_dungeon\_

says that when the player character arrives at the dungeon the quest item and its defender are hidden inside the dungeon for the player to find.

    waiting task:
        pc at \_dungeon\_ do hideItem

    hideItem task:
        place \_questItem\_ at \_dungeon\_
        place \_foe\_ at \_dungeon\_

accomplishes the same thing.

Writing a task when the player character isn't at a particular place is possible but rather tricky to avoid unwanted side-effects. You need to think through carefully what it means to have an action on-going the whole time the player isn't at a place. It may not be obvious but **X-engine** will schedule such a task every minute of the game when the player isn't at the specified place. The actions you can reasonably take are very limited. _Say_ something? Not unless you want the user to dismiss pop-up messages til kingdom come.

Some of the Bethesda quests describe tasks when the PC isn't at a place, but not often. Decompile the \_brisien quest if you're curious. Presumably the PC is not in the starter dungeon while using the character generator.

You can also detect when the PC is at the same place a quest NPC has been positioned. Usually you won't need this form since the place where the NPC is often has its own symbol. But you can use this form to check when the PC arrives at the birthplace of the NPC when your quest prefers to rely on such random selection.

* * *

### Noticing player discards

You can detect when the player character drops quest items from the inventory into the game world.

    dropped anItem at aPlace
    when anItem dropped at aPlace

schedules a task when the PC drops a quest item at a quest location defined by a Place command.

    Message: 1019
      <ce> You put the poisoned decanter back where you found it.

    when \_item\_ dropped at \_house\_
        say 1019
        end quest

A quest can detect when the player abandons a quest item via the _remove_ option on the inventory screen.

    Message: 1019
        <ce> Good.  You replace the poison-filled decanter.

    replaceItem task:
        dropped \_poison\_ at \_house\_
        say 1019

* * *

### Noticing spells cast

A quest can notice when the player character casts specific spells within the game.

    cast aSpell spell do aTask
    when aSpell spell cast

A quest can notice whether the player has cast one of the standard magic spells available in the game, and activate a task to take charge when that event occurs.

aSpell must be one of the stock spells from a spell vendor.

You cannot detect custom spells built by the spellmaker since there isn't any way for a prefabricated quest to know what combination of features a player may select through the spellmaker.

The names of the standard magic spells are shown in the table below. Not all spells can be purchased from the spell vendors. These spells are shown in _**italics**_.

**_Arsenic_**

Balyna's\_Antidote

Balyna's\_Balm

Banish\_Daedra

Buoyancy

Calm\_Humanoid

Chameleon

Charisma

Charm\_Mortal

Cure\_Disease

Cure\_Poison

_**Drothweed**_

Energy\_Leech

Far\_Silence

Feet\_of\_Notorgo

Fenrik's\_Door\_Jam

Fire\_Storm

Fireball

Force\_Bolt

Fortitude

Free\_Action

Frostbite

Gods'\_Fire

Hand\_of\_Decay

Hand\_of\_Sleep

Heal

Holy\_Touch

Holy\_Word

Ice\_Bolt

Ice\_Storm

_**Indulcet**_

Invisibility

Iron\_Will

Jack\_of\_Trades

Jumping

Levitate

Light

Lightning

_**Lycanthropy**_

_**Magebane**_

Magicka\_Leech

Medusa's\_Gaze

_**Moonseed**_

Nimbleness

Null\_Magicka

_**Nux\_Vomica**_

Open

Orc\_Strength

Paralysis

_**Pyrrhic\_Acid**_

_**Quaesto\_Vil**_

Quiet\_Undead

Recall

Resist\_Cold

Resist\_Fire

Resist\_Poison

Resist\_Shock

Shadow\_Form

Shalidor's\_Mirror

Shield

Shock

Silence

Sleep

Slowfalling

_**Somnalius**_

Soul\_Trap

Spell\_Absorption

Spell\_Drain

Spell\_Reflection

Spell\_Resistance

Spell\_Shield

Sphere\_of\_Negation

Spider\_Touch

Stamina

Strength\_Leech

_**Sursum**_

Tame

_**Thyrwort**_

Tongues

Toxic\_Cloud

Troll's\_Blood

Vampiric\_Touch

Water\_Breathing

Water\_Walking

Wildfire

Wisdom

Wizard's\_Fire

Wizard\_Lock

Wizard\_Rend

* * *

### Noticing player levels

A quest can notice whether the player has achieved a certain level-up in the game world.

    level nnnn completed

This condition is associated with Snnnnnnn quests. It is true if the player has achieved level nnnn at the present time within the game world. If so, the actions associated with this task are performed.

* * *

### Noticing several things together

The preceding conditions essentially describe a single event each in the game world. But from time to time a quest needs to take one action when several conditions are satisfied. We've seen the preliminaries in the condition for toting an item and clicking on an NPC. More elaborate chains of circumstance can be crafted.

    when aTaskName and ...
    when not aTaskName and ...
    when aTaskName or ...
    when not aTaskName or ...

Any named quest task can be checked to find out whether it has been performed or not. The **X-engine** allows up to four (4) tasks to be checked at the same time.

    Message: 1019
      <ce> You've slain the =foe\_.
      Here's your magic item reward.

    deadFoe task:
        killed \_foe\_

    variable haveItem

    reward task:
        when deadFoe and not haveItem
        get item \_reward\_
        say 1019
        have \_reward\_ set haveItem

which bestows a reward on the PC after killing a quest foe. Since the quest may not yet be finished, remember the gift in the _haveItem_ variable so the reward will only be given one time--which is probably what you want.

    Message: 1019
      <ce> You've slain enough enemies.
      Here's your magic item reward.

    deadFoe1 task:
        killed \_foe1\_

    deadFoe2 task:
        killed \_foe2\_

    deadFoe3 task:
        killed \_foe3\_

    variable haveItem

    finishQuest task:
        when deadFoe1 or deadFoe2 or deadFoe3
        get item \_reward\_
        say 1019
        emd quest

which bestows a reward on the PC if any one of three different foes have been slaughtered.

Using a combination of _ands_, _ors_, and _nots_ it is possible to construct a contingency from any of the preceding basic conditions, which can make for very interesting plot lines, especially when combined with the `[random task selection](#pickone)` command.

It is also possible to mention several different quest condtions together at the beginning of a task. When you do so, the actions of the task are scheduled if one or more of the conditions are satisfied.

    \_bigFinish\_ task:
         clicked \_item1\_
         clicked \_item2\_
         clicked \_item3\_
         killed \_foe\_
         end quest

which would stop the quest whenever the PC finds any one of three quest items or has killed the quest foe.

* * *

### Checking faction repute

    when faction n1 repute is at least n2

This condition checks whether the PC's repute with one of the **Daggerfall** factions exceeds a particular value.

Main quests use this condition to test the PC's repute with the principal NPC of the story before scheduling certain of the subplots for execution.

Some main quests do not declare a Person command for permanent NPCs that they wish to monitor. Instead the faction number of that permanent NPC is used to access the PC's repute.

Although the main story limits the use of this condition to permanent NPCs, checking the NPC's repute with any of the **Daggerfall** factions is likely to work.

* * *

### Checking faction availability

    when aFaction is available

This condition checks whether the specified faction is available for assignment as a questor in an upcoming quest that the present quest would like to schedule. nnn is either a faction number, or the proper name of a faction. See the `[_Person_](#persons)` command for the complete list of factions.

This condition is triggered by the player clicking the mouse on an NPC sprite associated with the specific faction in the game world.

The main quest monitors the availability of NPCs connected with the main story before attempting to schedule a new subplot. The player triggers this check by clicking on the NPC sprite that corresponds to the specified faction.

Some permanent NPCs play a role in several different subplots. To avoid inadvertently scheduling a new subplot which uses a permanent NPC in a starring role while the same NPC is already starring in an active quest, the monitor quest checks whether the desired NPC is free at the moment, to schedule their entrance in the upcoming subplot.
