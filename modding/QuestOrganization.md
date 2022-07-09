# Quest Organization

The source file created by the quest decompiler is divided into three parts:

*   Quest preamble
*   Qrc text blocks
*   Quest resources and operation (Qbn)

which are discussed subsequently.


## Quest Preamble

The quest preamble lays out the general requirements of the quest: who offers it, who can receive it, how it is started, etc. The purpose of this preamble is to collect the information required to fabricate a standard quest file name pattern. There are two ways to do so, concisely or verbosely.

Quest: pattern

where pattern is the desired quest file name pattern. For example

    Quest:  b0c00y13

Alternately, the pattern can be inferred by the compiler from the following verbose descriptions. Case is insignificant.

* * *

StartsBy: \[Letter | NPC\]

Indicates whether the quest starts by the player character receiving a letter or by contacting an NPC (non-player character) in the game.

* * *

Questee: \[Initiate | Member | Anyone\]

Indicates what general relationship the PC must have with respect to the NPC / group that offers the quest.

Initiate

Initial invitation to join

Member

PC must be a member of this group

Anyone

Anyone may be offered this quest



* * *

Questor: quest grantor or single letter key

The _questor_ is the group, guild, or daedra prince which offers the quest. It can be specified by single letter key (as used by the quest file name pattern) or by the proper name of the group, taken from the following list:

Questor

Key

Ids\[1\]

Questor

Key

Ids

Questor

Key

Ids

Akatosh

D

0

Lady Azura

T

0

Peasant

A

0-18

Arkay

E

0

Mage

N

1-18

Peryite

5

0

Boethiah

U

0

Main

S

\[2\]

Royalty

R

0-28

Brotherhood

L

0-11

Malacath

8

0

Sanguine

7

0

Clavicus Vile

V

0

Mara

H

0

Sheogorath

6

0

Dibella

F

0

Mehrunes Dagon

Y

0

Stendarr

I

0

Fighter

M

0-19

Mephala

Z

0

Temple

C

0-15

Hermaeus Mora

W

0

Merchant

K

0-10

Thief

O

0-12

Hircine

X

0

Meridia

1

0

Vaernima

9

0

Julianos

0

0

Molag Bal

2

0

Vampire

P

0-10

Knight

B

0-17

Namira

3

0

Witch

Q

0-8

Kynaran

G

0

Nocturnal

4

0

Zenithar

J

0


\[1\]See _QuestId:_ below. CompUSA quests are included.  
\[2\]Main story numbers in use are 1-22, 100-107, 500-503, 977, 988, and 999.

Again, case is not significant. Space, for those daedra princes with two names, _is_. Main refers to the main story quests.

     Questor: Arkay

indicates a quest offered by the temple of Arkay, for example.

* * *

Repute: mm

The repute is the minimum repute the PC must have with respect to the group / prince offering the quest. It can also indicate a range of reputes for which the quest is offered.

If mm is strictly numeric, then it indicates the minimum repute the PC must have, with 0 being the smallest repute and 99 the largest.

Quests can be restricted to a particular decade of the PC's repute with the questor by writing the units digit as the letter _x_.

       Repute: 3x

limits the quest to PCs whose repute is between 30 and 39 with the questor.

* * *

QuestId: nn

**X-engine** expects every quest associated with one of the questor groups to have a unique two-digit serial number assigned to it.

        QuestId: 17

assigns _17_ as the quest id in the file name pattern for the quest.

If the quest preamble describes a quest that already exists, the compiler will replace the original contents of that quest with the new contents from this source file.

If that quest does not already exist, the compiler will create a new **Daggerfall** quest for it.

Please see the table of guilds/questors for the quest ids already in use by the game.

* * *

Messages: aNumber

The quest compiler needs to know approximately how many message blocks will be recorded in the Qrc file before it begins processing. By default it reserves space for 50 message blocks. If the quest scenario supplies more than 50 messages, the compiler needs to be told the number of messages to expect with this directive.

The decompiler now automatically includes this directive when it decompiles a quest, as a convenience for recompiling quests. However, adding additional messages to existing quests may bump into the message count limit. The limit need not be exact, just more than enough.

* * *

AllowDupes:

Some existing quests from Bethesda have Qrc files that contain duplicate message block numbers. This is arguably wrong when you're creating a fresh quest, and the compiler normally smacks you if you try to do such a thing. But to allow the existing quests to recompile without trying to figure out which text block is the valid one--good luck--supplying this directive in the quest preamble will instruct the compiler not to whine if it finds message block numbers are being reused.

* * *

QuestDir: aDirectoryPath

You can tell the compiler where you want it to produce the qrc/qbn files by supplying the directory path name where you'd like them to go. By default, the compiler drops them in the same directory as the scenario source file.

        QuestDir:  d:\\games\\dag\\arena2

directs the compiler to the arena2 directory of my **Daggerfall** installation.

* * *

QRC Text Blocks
---------------



Every message displayed by a quest is assigned a serial number starting from 1000 and increasing. The first dozen numbers have stock behavior associated with them, and may be called for display automatically by the game engine during the quest, without an explicit trigger in the compiled quest script.

The text block section of the source file is announced by the directive:

    Qrc:

The compiler will expect to find text message blocks thereafter in the source file, until it is told otherwise. Again, the case of the directive doesn't matter.

In the source file, the text composing a quest message is introduced by the Message: directive followed by the message number to assign to the text block.

The compiler accepts synonyms for the stock quest messages:

QuestorOffer:  
Message: 1000

What the questor says when the PC makes contact for the quest. Although not enforced by the compiler, for quests started in person, this is likely a required message.

* * *

RefuseQuest:  
Message: 1001

What the questor says when the PC refuses the quest offer. Again, while the compiler is silent about its omission, for quests started in person, this is likely a required message block.

* * *

AcceptQuest:  
Message: 1002

What the questor says when the PC accepts the quest. Again, this block is likely expected for quests started in person.

* * *

QuestFailed:  
Message: 1003

What the questor says until the PC completes the quest. Again, this block is likely expected for quests started in person.

* * *

QuestComplete:  
Message: 1004

What the questor says when the PC completes the quest and returns. Again, this block is likely expected for quests started in person.

Note that certain quest actions will post this message automatically. See `[_give pc_](#givepc) / [_give pc nothing_](#givepc3)`.

* * *

RumorsDuringQuest:  
Message: 1005

Gives information that may come up during _Any News?_ dialogs the PC has during the quest.

* * *

RumorsPostFailure:  
Message: 1006

Gives information that may come up during _Any News?_ dialogs the PC has after failing the quest. The **X-engine** eventually retires these after a suitable time lag.

* * *

RumorsPostSuccess:  
Message: 1007

Gives information that may come up during _Any News?_ dialogs the PC has after completing the quest. The **X-engine** eventually retires these after a suitable time lag.

* * *

QuestorPostSuccess:  
Message: 1008

What the questor says to the PC in dialog after a successful quest. That is, when the PC chats with the questor through the dialog screen.

* * *

QuestorPostFailure:  
Message: 1009

What the questor says to the PC in dialog after a failed quest. That is, when the PC chats through the dialog screen.

* * *

QuestLogEntry:  
Message: 1010

The message inserted into the PC's log when the quest is accepted. Probably not used automatically.

* * *

QuestTimeLapse:  
Message: 1045

Displayed when the PC fails to meet a quest with a dead-line. Probably not used automatically.

* * *

The quest author can create as many additional message blocks as the quest requires, although the compiler arbitrarily complains if you choose message numbers larger than 2100. 1000 message blocks isn't a quest, it's a whole game.

Since remembering what text is associated with a Qrc message number is tedious, the compiler will accept an arbitrary label in place of a message number. The compiler will automatically assign the next available message number to each message label you provide. And you can refer to the Qrc message by that label in the Qbn section of the quest file.

    Message: NpcPeeved
    What? I can't believe you took so long
    for such a simple request!

You can mix labels with section numbering in different message blocks.

Following the Message: directive are the lines composing that message block. By default, left justification is used.

Text can be centered by using the <ce> tag at the beginning of the line to be centered.

An empty or blank line can be inserted by using the <br> tag for the content of a line.

A message block may be divided into subsections. When **X-engine** displays a subsectioned message block, it displays only one of the available subsections of that block at a time. The selected subsection is chosen at random from the subsections available for the block. The next time **X-engine** is asked to display that message block it repeats this process, and so may select a different subsection for display.

Subsectioning gives variety to the messages that appear when the PC performs the same quest more than once. Indeed, with clever use, the quests can appear quite different when coupled with the randomizing feature in the Qbn section. While there may be an upper bound on the number of subsections the **X-engine** can handle, there are existing quests with upwards of a dozen subsections each in several different message blocks.

Any message block can be subsectioned by using the <---> tag on a line to start a new subsection:

    RumorsDuringQuest:
    <ce>I heard that %ra is a complete loser.
                <--->
    That %ra is after some loser's hide.
                <--->
    <ce>Isn't %ra chasing after \_questgiver\_ for something?

The position of the tag on the line is irrelevant.

    RumorsDuringQuest:
           <ce>I heard that %ra is a complete loser.
    <--->
    That %ra is after some loser's hide.
    <--->
       <ce>Isn't %ra chasing after \_questgiver\_ for something?

works just as well. The decompiler takes advantage of this to center the decompiled text as required.

Be careful of leaving blank lines between different quest message blocks unless you intend the message to display empty lines in the block. Generally, you want only one blank line between the end of one block and the next message block:

    RumorsDuringQuest:
    Very strange sounds come out of the \_house\_.
            <--->
    \_qgiver\_'s house has really gone to Oblivion.

    RumorsPostfailure:
    Poor \_qgiver\_ has to hire an exterminator.
            <--->
    It's going to cost \_qgiver\_ to clean that house.

* * *

### Qrc Symbols

**X-engine** likes to name replaceable parts of text as \_symbol\_, where in fact symbol can be any meaningful mnemonic. (See `[Quest Resources](#qbn)` for how symbols are created)

There are some fairly intricate rules for using symbols in a Qrc message block.

\_foo\_ is replaced with the name of foo: a ring, an NPC's name, or kind of enemy. It is generally an error to use this form for a Place or Clock quest resource. The quest compiler won't stop you from doing so, but the results within the game world aren't likely to be what you expect.

\=foo\_ is replaced by the clock time (expressed as the number of days that the clock will be active) when \_foo\_ is a Clock quest resource. Or by the character class when \_foo\_ is an NPC quest resource, or by the foe's name for enemy resource.

\==foo\_ is replaced by the faction association for an NPC quest resource. So for a Person symbol:

  \_foo\_ of ==foo\_

would result in something like

  Zaphod Beeblebrox of the Blades

while

  \_foo\_ the =foo\_

would result in something like

  Zaphod Beeblebrox the Acrobat

If \_foo\_ is a Person resource then \_\_\_foo\_ (_three_ leading underscores) gives the town name where \_foo\_ can be found. While \_\_foo\_ (_two_ leading underscores) gives the name of the house/shop in that town where \_foo\_ can be found. If you move \_foo\_ to different places in the game world, it isn't obvious that these symbols will track those moves. I tend to regard them as 'birthplace' markers, rather than present residence markers.

Place symbols follow slightly different semantics.

\_foo\_ is the form of a shop name.

\_\_foo\_ is the town where the shop can be found.

\_\_\_foo\_ is the form of a dungeon name.

\_\_\_\_foo\_ is the name of the province where the place can be found.

This pattern holds regardless of whether \_foo\_ was created as a shop or as a dungeon. This is why some existing quests give out mysterious hints about non-existent shops. The quest author left out underscores in the Qrc text, and got the shop name format instead of the dungeon name format.

Note that quest locations which are not randomly generated, the _permanent_ sites, do not follow these rules at all. The symbol used to represent a permanent site is always replaced by random garbage in a Qrc block. So the information for a permanent site must needs be written out in full inside a Qrc block.

The following table summarizes the usage. **NO** means you probably don't want to use that substitution. At any rate, there aren't any such examples in the existing quests.

Symbol

Item

Person

Place\[1\]

Foe

Clock

\_foo\_

_ring_

_Zaphod Beeblebrox_

_Lord Marcus' Arms_

_warrior_

**NO**

\_\_foo\_

**NO**

_Adams residence_

_Daggerfall City_\[2\]

**NO**

**NO**

\_\_\_foo\_

**NO**

_Gothway Gardens_

_Ruins of Bethsoft_

**NO**

**NO**

\_\_\_\_foo\_

**NO**

_NO_

_Daggerfall_

**NO**

**NO**

\=foo\_

**NO**

_Acrobat_

**NO**

_Arthur Dent_

_4 days_

\==foo\_

**NO**

_Blades_

**NO**

_Fighter_

**NO**

\[1\]Not available for _permanent_ sites. \[2\]This form is invalid for remote dungeons.

There is a whole family of %symbols to scrape out information about the PC, NPC gender, and the like, which are tabulated below. \[1\]

The pronomial forms (_%g_, _%g1_, etc) refer _back_ to the last NPC or foe symbol appearing in the same Qrc text block to determine the proper declension. It is usually an error in content to use one of them prior to the first appearance of the proper noun you want them bound to. When a computer refers back that way, it's anybody's guess which person will actually be used for the construction. That happens from time to time in the existing quests, and so you may see quirky constructs like "...her research lab where Andystair Bonedoggle..." when you're pretty sure _Andystair_ takes the masculine, not feminine gender.

All of these symbols appear at one time or another, either in arena2\\text.rsc, fall.exe, arena2\\\*.qrc, or arena2\\bio\*.txt. It is unclear whether their use is restricted to contexts appropriate to each of those file sets, or whether you're free to mix bio\*.txt symbols within Qrc blocks. If you use one of these symbols in an improper context, Daggerfall generally posts an error message naming the offensive symbol and dies.

Symbols in _italics_ appear in the existing qrc files from Bethesda.

%1am

1st + Magnitude

%1bm

1st base Magnitude

%1com

Greeting (?)

%1hn

?

%2am

2nd + Magnitude

%2bm

2nd Base Magnitude

%2com

?

%2hn

?

%3hn

?

%a

Cost of somthing.

%ach

\+ Chance per

%adr

?

%adr

\+ Duration per

%agi

Amount of Agility

%arm

Armour

%ark

?

%ba

Book Author

%bch

Base chance

%bdr

Base Duration

%bn

?

%bt

Book title

%cbl

Cash balance in current region

%clc

Per level (Chance)

%cld

Per level (Duration)

%clm

Per level (Magnitude)

_**%cn**_

Current City

%cn2

?

%cpn

Current shop name

%cri

Accused crime

%crn

Current Region

%dae

A daedra

%dam

Damage modifyer

_**%dat**_

Date

_**%di**_

Direction

%dip

?

%dng

Dungeon

%dts

Daedra

%dwr

?

%ef

Local shop name

%enc

Encumberence

%end

Amount of Endurance

%fcn

Another city

%fe

?

%fea

?

%fl1

Lord of %fx1

%fl2

Lord of %fx2

%fn

Random first(?) name (Female?)

%fn2

Same as %mn2 (?)

%fnpc

?

%fon

?

%fpa

?

%fpc

?

%fx1

A faction in news

%fx2

Another faction in news

_**%g**_

He/She etc...

_**%g1**_

He/She ???

_**%g2**_

Him/Her etc...

_**%g2self**_

Himself/Herself etc...

_**%g3**_

His/Hers/Theirs etc...

%gii

Amount of gold in hand

_**%god**_

Some god (listed in TEXT.RSC)

%gtp

Amount of fine

%hea

HP Modifier

%hmd

Healing rate modifer

%hnr

Honorific

%hnt

Direction of location.

%hnt2

?

%hol

Holiday

%hpn

?

%hpw

?

%hrg

?

%hs

Holding Soul type

%htwn

?

%imp

?

%int

Amount of Intelligence

%it

Item

_**%jok**_

A joke

%key

A location (?)

%key2

Another location

%kg

Weight of items

_**%kno**_

A knightly guild name

%lev

Rank in guild that you are in.

%ln

Random lastname

%loc

Location marked on map

%lt1

Title of %fl1

%ltn

In the eyes of the law you are.......

%luc

Luck

%map

?

%mad

Resistance

%mat

Material

%mit

Item

%mn

Random First(?) name (Male?)

%mn2

Same as %mn (?)

%mod

Modification

%mpw

Magic powers

_**%n**_

A random female first name

_**%nam**_

A random full name

_**%nrn**_

Noble of the current region

%nt

?

%ol1

Old lord of %fx1

%olf

What happened to %ol1

%on

?

_**%oth**_

An oath (listed in TEXT.RSC)

%pc

?

_**%pcf**_

Character's first name

_**%pcn**_

Character's full name

_**%pct**_

Character's class

%pdg

Days in jail

%pen

Prison sentence

%per

Amount of Personality

%plq

Place of something in log.

%pnq

Person of something in log

%pp1

?

%pp2

?

%pqn

Potential Quest Giver

%pqp

Potential Quest Giver's Location

%ptm

An enemy of the current region (?)

%q1 to %q12

Effects of questions answered in bio.

_**%qdt**_

Quest date of log entry

_%qdat_

Quest date of log entry \[2\]

%qot

The log comment

%qua

Condition

%r1

Commoners rep

%r2

Merchants rep

%r3

Scholers rep

%r4

Nobilitys rep

%r5

Underworld rep

_**%ra**_

Player's race

_**%reg**_

Region

_**%rn**_

Regent's Name

_**%rt**_

Regent's Title

%spc

Current Spell Points

%ski

Skill

%spd

Speed

%spt

?

%str

Amount of strength

%sub

?

**_%t_**

Regent's Title

%tcn

?

%thd

Combat odds

%tim

Time

_**%vam**_

PC's vampire clan

%vcn

Vampire's Clan

%vn

?

%wdm

Weapon damage

%wep

Weapon

%wil

?

%wpn

Poison (?)

%wth

Worth

\[1\] Reproduced from Appendix A, _Daggerfall Quest File (QRC/QBN) Hacking Results - v1.00_, by Dave Humphrey `[\[email protected\]](/cdn-cgi/l/email-protection)` I've added 2 or 3 additional symbols from the qrc files.  
\[2\] Appears in _d0b00y00_, the Akatosh Chantry quest, but looks like a simple misspelling of _%qdt_.

* * *

Quest Resources and Operations
------------------------------

Once the text blocks for the quest have been laid out, the contents of the Qbn file are described by using the Qbn directive

    Qbn:

to instruct the compiler to finish the Qrc section and begin the Qbn section of the source code.

The Qbn section is by far the more complicated part, and is itself further divided into subsections:

*   Items required in the quest
*   Persons encountered in the quest
*   Places encountered in the quest
*   Clocks or timers used in the quest
*   Foes encountered in the quest
*   Condition/responses that operate during the quest

The exact order you pick is somewhat irrelevant. The only requirement by the compiler is that the condition/responses appear last, since they generally refer to resources defined by the other sections.

* * *

### Managing the Dialog Screen

The scenario can control whether to include each quest item, person, or place resource in the pick list under the _Tell me about..._ dialog tab.

Each item, person, or place resource can be embellished with the tag

    anyInfo aQrcNumber

where aQrcNumber is a Qrc message block to display in the dialog screen when the PC chats about that particular resource.

When the scenario assigns an _anyInfo_ Qrc message to a quest resource, the name of that resource automatically appears as a new entry in the pick list under the _Tell me about..._ dialog tab.

Such entries are available throughout the duration of the quest for use in conversation with anyone the PC happens upon in the game world.

Note, however, that the PC's repute with the NPC being spoken to affects the outcome of chatting about such entries. So not every NPC spoken to will offer the information provided by the scenario when quizzed by the PC. But, when combined with the subsectioning message block technique, different pieces of information may be supplied at each chat session.

But sometimes a scenario wants to make such information available only after the PC has met certain milestones within the quest. In such cases, the scenario can control when the _anyInfo_ messages become available through the `[_dialog link_](#rumor5) and [_add dialog_](#rumor1)` family of quest actions.

Note that once an entry has been added to the _Tell me about..._ pick list, it is stuck there for the remaining life of the quest. There does not appear to be any quest action to delete entries from the list.

A person or place quest resource may be further embellished with the tag

    rumors aQrcNumber

where aQrcNumber is a Qrc message block to display in the dialog screen when the PC selects the _Any News?_ dialog option.

Oftentimes, adding a _rumors_ tag will insert the quest resource into the dialog picklist as well, but this insertion can be suppressed with `[_dialog link_](#rumor5)` family of commands.

* * *

### Quest Items

Every item associated with the quest must be described for the **X-engine** using a form of the Item command. Items include not only things, like rings, letters, drugs, or artifacts, but also quest rewards like gold payment.

In addition each item can have message text from the Qrc file associated with it, which is displayed when the item is discussed with an NPC or when the PC 'uses' the item in the inventory screen.

Quest items with usage text will also appear in the About picklist when conversing with NPCs. The Qrc text will be 'spoken' by the NPC if asked about the item.

Whether the item automatically appears as a topic under the _Tell me about..._ dialog tab is somewhat variable. Non-letter items tend always to appear, while letter items tend to depend upon their method of delivery to the PC.

Regardless, the scenario can regulate the contents of the _Tell me about..._ picklist with the `[_dialog link_](#rumor5) and [_add dialog_](#rumor1)` actions.

To prevent a quest item with usage text from appearing in the dialog picklist, simply include a _dialog link item_ for it in the quest start-up. If there is no corresponding _add dialog item_ action for it, then it will never appear.

As a rule, when the PC acquires a quest item in the inventory it shows up with a green background. Furthermore, such items will be automatically taken from the player when the quest terminates. Special steps have to be taken in the condition/response section to bestow an item (like an artifact) on the player permanently.

So in general, don't expect a quest item to hang around after the player completes the quest unless you provide for that specially in the condition/response section.

Also note that quest items are unknown to **X-Engine** until the scenario puts them somewhere in the game world. The practical consequence of this is that displaying a Qrc message block referring to a quest item _before_ that item has been placed in the world, or _after_ it has been destroyed, will most likely result in the infamous BLANK text when the Qrc message is shown to the player.

A quest isn't required to have a item section, but most quests that do anything interesting have them.

There are three general forms for the Item command:

*   reward (gold) items
*   artifact items
*   other items

* * *

### Reward items

    Item  \_symbol\_  gold
    item  \_symbol\_  gold  range  nn  to  mm

The first form creates a random amount of gold reward proportional to the player's current level. While the relation isn't exactly linear in player level, expect about 100 times the player's level when the quest begins. So for a level 2 player, expect the gold reward to be in the neighborhood of 200 gold.

\_symbol\_ is an arbitrary name to refer to the item throughout the quest scenario. It must be unique among all the item resources--the compiler will smack you if it isn't--and it is helpful to make it unique to all the symbol names you use in the Qbn section, as well as indicative of what the item is:

    Item \_reward\_ gold
    Item \_bribe1\_ gold range 50 to 100

The second form of the reward item selects a random amount within the requested range. If the PC repeats the quest, that attempt won't necessarily have exactly the same reward amount.

This isn't always a bargain. Some players react to this with "Goody, it's different this time." But others react with "Boo! This quest isn't the same! What's wrong now?"

To make a reward of an exact amount, use the same number for the beginning and end of the range.

Any Item command can be embellished to provide a message when the player chats about the item with another NPC.

    Item \_reward\_ gold anyInfo 1017

requests the **X-engine** to display Qrc message block 1017 when the gold reward is discussed with another NPC.

Every item command can be embellished to provide a message when the item is 'used' in the player's inventory screen.

    Item \_letter1\_ letter used 1022

requests the **X-engine** to display Qrc message block 1022 when the item is used in the player's inventory screen.

The two embellishments can be combined:

    Item \_letter1\_ letter anyInfo 1014 used 1022

* * *

### Artifact items

    Item  \_symbol\_  artifact  artifactName

describes an artifact item appearing in a quest. artifactName must be one of the known Tamriel artifacts:

Auriels\_Bow

Oghma\_Infinium

Auriels\_Shield

Ring\_of\_Khajiit

Azuras\_Star

Ring\_of\_Namira

Chrysamere

Sanguine\_Rose

Ebony\_Blade

Skeletons\_Key

Ebony\_Mail

Skull\_of\_Corruption

Hircine\_Ring

Spell\_Breaker

Lords\_Mail

Staff\_of\_Magnus

Mace\_of\_Molag\_Bal

Volendrung

Masque\_of\_Clavicus\_Vile

Wabbajack

Necromancers\_Amulet

Warlocks\_Ring



As usual, case doesn't matter, but notice underscores (\_) used instead of blanks. You can, however, change the names to your own liking by editing items.src. Indeed you can change most of the compiler's notion of spelling by editing any of the \*.src files to your own preferences.

The artifact item command can be embellished with an anyInfo notice. Some item artifacts in the existing quests from Bethesda are embellished with the used tag, but this is inconsistent. When an artifact is used in the inventory screen, the message displayed is invariably taken from TEXT.RSC, not the Qrc block.

* * *

### Other items

    Item  \_symbol\_  someItemName
    Item  \_symbol\_  item  class nn  subclass mm

Some common items are prefabricated in the items.src data base. **Daggerfall** also uses a sort of generic item specification, to select randomly one item from a group of related items. These forms are shown in **_italics_** in the following table.

aegrostat

amulet

armbands

**_armor_**

**_book_**

book0

book1

book2

book3

boots

bracelet

bracer

cloth\_amulet

coins

common\_symbol

daedra\_heart

decanter

**_deed_**

diamond

**_drug_**

**_element_**

emerald

finger

**_flamable_**

**_gem_**

gold\_bar

harpy\_feather

horn

indulcet

ivory

jade

**_junk_**

kimono

lantern

**_large\_plant_**

large\_sack

letter

lich\_dust

lodestone

**_magic\_item_**

malachite

mantella\_crux

**_map_**

mark

**_mens\_clothing_**

**_mineral_**

**_misc_**

mummy\_wrappings

**_mythic_**

**_organs_**

**_painting_**

pearl

portrait

quaesto\_vil

random\_map

random\_recipe

**_religious_**

ring

root\_tendrils

ruby

sapphire

scarab

**_skin_**

**_small\_plant_**

small\_sack

snake\_venom

**_specials_**

straps

sursam

talisman

telescope

torc

totem

**_transport_**

**_trinket_**

turquoise

tusk

wand

**_weapon_**

werewolf\_blood

**_womens\_clothing_**

womens\_robe

wraith\_essence

yellow\_flowers



Other items can be created at will by using the class/subclass form to describe any **Daggerfall** item.

Again, the item command can be further embellished with an anyInfo notice and/or a used notice.

You can find tables of the known **Daggerfall** item codes at the _Unofficial Elder Scrolls' Pages_.

* * *

### Quest Persons

Each NPC involved in a quest has to be described in the Qbn section with a Person command. Most quests will have at least one Person command to describe the questor NPC granting the quest to the player. This appears to be how the **X-engine** couples the change of repute to the allied/enemy factions associated with the questor when the PC completes or fails the quest.

Factional alliances / repudiations are fairly involved in **Daggerfall**. This document won't treat all aspects of factions or how to choose correct factions for a quest. Basically it just depends on the goals of the quest.

Briefly, the **Daggerfall** social structure forms a four-tier hierarchy of Guild-Group, Social Group, Faction Type, Individual-Faction. Unfortunately I haven't been able to determine the magic numbers used to describe Social Groups in the Qbn files. The elements of the remaining classifications are, however, known.

For example, Clavicus Vile, a Daedra, Supernatural Being, of Oblivion. The guild-group name is _Oblivion_, the social group is _Supernatural Being_, the faction-type is _Daedra_, and the individual-faction is _Clavicus Vile_.

Or another example, The Battlelords subgroup of the Guild Members of the Knightly Order. The guild-group name is _Knightly Order_, the social group is _Guild Members_, the faction-type is _Subgroup_, and the individual-faction is _The Battlelords_.

All the non-permanent NPCs in the game can be so classified.

Permanent NPCs are slightly different, in that they have no top level guild-group name. For example, King Gothryd, a person of nobility. The social group is _nobility_, the faction-type is _person_, and the individual-faction is _King Gothryd_.

For the nonce, NPCs can be selected by faction type, guild group, individual faction, or permanent NPC.

As with _Items_, message blocks can be associated with NPCs in two different ways.

anyInfo nn adds the Qrc message _nn_ to the Tell me about... dialog responses under the NPC's name.

rumors nn adds the Qrc message _nn_ as a possible response when gossiping in the dialog panel under _Any News?_. Under patch 213 it also adds the NPC's name to the dialog list of choices, even if there is no anyInfo nn entry for the NPC.

These tags can be appended to any Person command.

* * *

### Permanent NPC

Basically, a permanent NPC is one of the named major figures of the royal houses in Daggerfall, Sentinel, or Wayrest, or supernatural figures like the King of Worms. They are all generally involved in the main quest in one way or another.

The Person command tags a permanent NPC with the named field, since all permanent NPCs have invarient names within the game.

Permanent NPCs can be used in one of two ways. They can be located at their regular place in the game world, which the Person command tags as atHome. So the player would encounter a permanent NPC at their normal place in the game world.

Alternatively, the permanent NPC can be positioned elsewhere in the Bay area for the duration of the quest. The **X-engine** needs to be told up front which you plan to do so it can do all the necessary bookkeeping to keep the NPC portraits managed correctly.

Be aware that fiddling with the NPCs associated with the main story in side-quests can have surprising side-effects. Recall that it is always possible for the PC to have several quests operating concurrently.

It's even possible to have multiple copies of the same side-quest active at the same time. Consider the typical mage guild quest to fetch a rare ingredient from a dungeon. It's perfectly possible to pick up that quest from the guild in Wayrest, say, and immediately teleport to Sentinel and acquire the same quest from the guild hall there.

You need to consider consequences of that sort if you use permanent NPCs in a quest offered from more than one location in the game world. It probably isn't realistic to expect the **X-engine** to situate the King of Worms in two different places at the same time.

The quest condition `[_faction available_](#factionfree)` can help to determine whether a permanent NPC is presently starring in another quest.

    Person  \_symbol\_  named  aPermanentNPC
    Person  \_symbol\_  named  aPermanentNPC  atHome

The complete list of permanent NPCs is shown below.

Azura

Baltham\_Greyman

Baron\_Shrike

Baroness\_Dh'emka

Boethiah

Br'itsa

Charvek-si

Chulmore\_Quill

Clavicus\_Vile

Farrington

Gortwog

Hermaeus\_Mora

Hircine

Karethys

King\_Eadwyre

King\_Gothryd

King\_of\_Worms

Lady\_Bridwell

Lady\_Brisienna

Lady\_Doryanna\_Flyte

Lord\_Auberon\_Flyte

Lord\_Bertram\_Spode

Lord\_Bridwell

Lord\_Castellian

Lord\_Coulder

Lord\_Darkworth

Lord\_Harth

Lord\_K'avar

Lord\_Khane

Lord\_Kilbar

Lord\_Perwright

Lord\_Plessington

Lord\_Provlith

Lord\_Quistley

Lord\_Vhosek

Lord\_Woodborne

Malacath

Medora

Mehrunes\_Dagon

Mephala

Meridia

Mobar

Molag\_Bal

Mynisera

Namira

Nocturnal

Nulfaga

Orsinium

Peryite

Popudax

Prince\_Greklith

Prince\_Helseth

Prince\_Lhotun

Princess\_Elysana

Princess\_Morgiah

Queen\_Akorithi

Queen\_Aubk-i

Queen\_Barenziah

Sanguine

Sheogorath

Skakmat

Sylch\_Greenwood

Thaik

The\_Crow

The\_Night\_Mother

The\_Squid

The\_Underking

Thyr\_Topfield

Vaernima

Whitka

    Person \_qMother\_ named mynisera

creates the queen mother Mynisera as an NPC in a quest. She'll be teleported to a remote spot by the start-up task of the quest.

Note that you may easily confuse the player if your quest, say, teleports the King of Worms away somewhere while the player is off to fetch the lich soul for him as part of the main story. On the other hand, it might serve an interesting twist to the main plot.

* * *

### Faction Type

An NPC can be created with an affiliation with one of the twenty-odd factions to control how the success or failure of the player in the quest is spread among the Iliac Bay inhabitants.

    Person  \_symbol\_  factionType  aFactionName
    Person  \_symbol\_  factionType  aFactionName  female
    Person  \_symbol\_  factionType  aFactionName  male

where aFactionName is one of the known faction types:

Court

People

Daedra

Person

Generic\_Group

Region

God

Subgroup

Group

Temple

Knightly\_Guard

Thieves\_Den

Magic\_User

Vampire\_Clan

Official

Witches\_Coven



You can use the generic name Faction to select a faction at random when the quest begins.

The first form of the Person command picks the gender of the NPC at random, while the latter two forms request a specific gender.

anyInfo nn and rumors nn tags can be appended to supply message block numbers to use in the dialog screen.

    Person \_npc1\_ factionType temple male

would select a male NPC allied with the temple crowd.

* * *

### Group Alliances

An NPC can be created with an affiliation for one of the occupations found in Tamriel.

    Person  \_symbol\_  group  aGroupName
    Person  \_symbol\_  group  aGroupName  female
    Person  \_symbol\_  group  aGroupName  male

where aGroupName is one of the social groups/guilds:

Armorer

Jeweler

Resident3

Banker

Librarian

Resident4

Bookseller

Noble

Shopkeeper

Carpenter

Pawnbroker

Smith

Chemist

Questor

Spellcaster

Cleric

Resident1

Tailor

Innkeeper

Resident2



By coordinating the occupation of the NPC with site selection, you can control the venue that your NPCs appear in. Selecting a spellcaster as an NPC and a mage's guild hall for a site ensures the resulting sprite will be one the player is accustomed to finding in that locale. See `[_Local and Remote Sites_](#localsites)` for additional information.

The first form of the Person command picks the gender of the NPC at random, while the later two forms request a specific gender.

anyInfo nn and rumors nn tags can be appended to supply message block numbers to use in the dialog screen.

    Person \_npc1\_ group pawnbroker

would select an NPC allied with the pawnbrokers. The gender of the NPC is taken at random.

* * *

### Faction Alliances

An NPC can be created with an affiliation with a specific faction in the game.

    Person  \_symbol\_  faction  aFactionName
    Person  \_symbol\_  faction  aFactionName  female
    Person  \_symbol\_  faction  aFactionName  male

where aFactionName is one of the 400-odd factions from the table below.

Abibon-Gora

Agents\_of\_The\_Underking

Akatosh

Alcaire

Alik'ra

Anticlere

Antiphyllos

Apothecaries\_of\_Akatosh

Apothecaries\_of\_Arkay

Apothecaries\_of\_Dibella

Apothecaries\_of\_Mara

Apothecaries\_of\_Stendarr

Apothecaries\_of\_Z'en

Arkay

Ayasofya

Bergama

Betony

Bhoraine

Binders\_of\_Arkay

Children

Court\_of\_Abibon-Gora

Court\_of\_Alcaire

Court\_of\_Alik'ra

Court\_of\_Anticlere

Court\_of\_Antiphyllos

Court\_of\_Ayasofya

Court\_of\_Balfiera

Court\_of\_Bergama

Court\_of\_Betony

Court\_of\_Bhoraine

Court\_of\_Cybiades

Court\_of\_Daenia

Court\_of\_Daggerfall

Court\_of\_Dak'fron

Court\_of\_Dragontail

Court\_of\_Dwynnen

Court\_of\_Ephesus

Court\_of\_Gavaudon

Court\_of\_Glenpoint

Court\_of\_Glenumbra\_Moors

Court\_of\_Ilessen\_Hills

Court\_of\_Kairou

Court\_of\_Kambria

Court\_of\_Koegria

Court\_of\_Koegria

Court\_of\_Kozanset

Court\_of\_Lainlyn

Court\_of\_Menevia

Court\_of\_Mournoth

Court\_of\_Myrkwasa

Court\_of\_Northmoor

Court\_of\_Orsinium

Court\_of\_Phrygia

Court\_of\_Pothago

Court\_of\_Santaki

Court\_of\_Satakalaam

Court\_of\_Sentinel

Court\_of\_Tigonus

Court\_of\_Totambu

Court\_of\_Tulune

Court\_of\_Urvaius

Court\_of\_Wayrest

Court\_of\_Wrothgaria

Court\_of\_Ykalon

Crafters\_of\_Julianos

Cybiades

Cyndassa

Daenia

Daggerfall

Dak'fron

Dancers

Dark\_Binders

Dark\_Mixers

Dark\_Plotters

Dark\_Slayers

Dark\_Trainers

Default

Dibella

Dragontail

Dwynnen

Ebonarm

Enchanters\_of\_Kynareth

Ephesus

Fighter\_Equippers

Fighter\_Questers

Fighter\_Trainers

Gavaudon

Generic\_Knightly\_Order

Generic\_Temple

Glenpoint

Glenumbra\_Moors

Healers

Ilessan\_Hills

Isle\_of\_Balfiera

Julianos

Kairou

Kambria

Koegria

Kozanset

Kynareth

Lady\_Northbridge

Lainlyn

Mara

Menevia

Mixers\_of\_Akatosh

Mixers\_of\_Arkay

Mixers\_of\_Dibella

Mixers\_of\_Mara

Mixers\_of\_Stendarr

Mixers\_of\_Z'en

Mournoth

Myrkwasa

Northmoor

Oblivion

Orsinium

People\_of\_Abibon-Gora

People\_of\_Alcaire

People\_of\_Alik'ra

People\_of\_Anticlere

People\_of\_Antiphyllos

People\_of\_Ayasofya

People\_of\_Balfiera

People\_of\_Bergama

People\_of\_Betony

People\_of\_Bhoraine

People\_of\_Cybiades

People\_of\_Daenia

People\_of\_Daggerfall

People\_of\_Dak'fron

People\_of\_Dragontail

People\_of\_Dwynnen

People\_of\_Ephesus

People\_of\_Gavaudon

People\_of\_Glenpoint

People\_of\_Glenumbra\_Moors

People\_of\_Ilessen\_Hills

People\_of\_Kairou

People\_of\_Kambria

People\_of\_Koegria

People\_of\_Koegria

People\_of\_Kozanset

People\_of\_Lainlyn

People\_of\_Menevia

People\_of\_Mournoth

People\_of\_Myrkwasa

People\_of\_Northmoor

People\_of\_Orsinium

People\_of\_Phrygia

People\_of\_Pothago

People\_of\_Santaki

People\_of\_Satakalaam

People\_of\_Sentinel

People\_of\_Tigonus

People\_of\_Totambu

People\_of\_Tulune

People\_of\_Urvaius

People\_of\_Wayrest

People\_of\_Wrothgaria

People\_of\_Ykalon

Phrygia

Pothago

Questers

Random\_Knight

Random\_Noble

Random\_Ruler

Santaki

Satakalaam

Secret\_of\_Oblivion

Seneschal

Sentinel

Shalgora

Smiths

Smiths\_of\_Julianos

Spellsmiths\_of\_Kynareth

Stendarr

Summoners\_of\_Akatosh

Summoners\_of\_Arkay

Summoners\_of\_Dibella

Summoners\_of\_Julianos

Summoners\_of\_Kynareth

Summoners\_of\_Mara

Summoners\_of\_Stendarr

Summonists\_of\_Z'en

Teachers\_of\_Akatosh

Teachers\_of\_Arkay

Teachers\_of\_Dibella

Teachers\_of\_Julianos

Teachers\_of\_Kynareth

Teachers\_of\_Mara

Teachers\_of\_Stendarr

Teachers\_of\_z'en

Temple\_Blessers

Temple\_Healers

Temple\_Missionaries

Temple\_Treasurers

The\_Academics

The\_Acolyte

The\_Akatosh\_Chantry

The\_Anthotis

The\_Archmagister

The\_Bards

The\_Battlelords

The\_Beldama

The\_Benevolence\_of\_Mara

The\_Blades

The\_Cabal

The\_Citadel\_of\_Ebonarm

The\_Crafters

The\_Crow

The\_Crusaders

The\_Daggerfall\_Witches

The\_Dark\_Brotherhood

The\_Daughters\_of\_Wroth

The\_Dust\_Witches

The\_Fey

The\_Fighters\_Guild

The\_Garlythi

The\_Glenmoril\_Witches

The\_Great\_Knight

The\_Guildmagister

The\_Haarvenu

The\_Host\_of\_the\_Horn

The\_Host\_of\_the\_True\_Horn

The\_House\_of\_Dibella

The\_Isolationists

The\_Khulari

The\_Knights\_Mentor

The\_Knights\_of\_Iron

The\_Knights\_of\_the\_Circle

The\_Knights\_of\_the\_Dragon

The\_Knights\_of\_the\_Flame

The\_Knights\_of\_the\_Hawk

The\_Knights\_of\_the\_Owl

The\_Knights\_of\_the\_Rose

The\_Knights\_of\_the\_Wheel

The\_Kynaran\_Order

The\_Lyrezi

The\_Mages\_Guild

The\_Maran\_Knights

The\_Master\_at\_Arms

The\_Master\_of\_Academia

The\_Master\_of\_Incunabula

The\_Master\_of\_Initiates

The\_Master\_of\_Initiates2

The\_Master\_of\_the\_Scry

The\_Mercenary\_Mages

The\_Merchants

The\_Montalion

The\_Mountain\_Witches

The\_Necromancers

The\_Odylic\_Mages

The\_Oracle

The\_Order\_of\_Arkay

The\_Order\_of\_the\_Candle

The\_Order\_of\_the\_Cup

The\_Order\_of\_the\_Hour

The\_Order\_of\_the\_Lamp

The\_Order\_of\_the\_Lily

The\_Order\_of\_the\_Raven

The\_Order\_of\_the\_Scarab

The\_Palatinus

The\_Patricians

The\_Prostitutes

The\_Quill\_Circus

The\_Resolution\_of\_Z'en

The\_Royal\_Guard

The\_Royal\_Guards

The\_School\_of\_Julianos

The\_Selenu

The\_Septim\_Empire

The\_Shadow\_Appraisers

The\_Shadow\_Schemers

The\_Shadow\_Spies

The\_Shadow\_Trainers

The\_Sisters\_of\_Kykos

The\_Sisters\_of\_the\_Bluff

The\_Skeffington\_Witches

The\_Tamarilyn\_Witches

The\_Temple\_of\_Kynareth

The\_Temple\_of\_Stendarr

The\_Thieves\_Guild

The\_Thrafey

The\_Tide\_Witches

The\_Travelers\_League

The\_Utility\_Mages

The\_Vraseth

The\_Witches\_of\_Alcaire

The\_Witches\_of\_Devilrock

The\_Witches\_of\_the\_Marsh

Thyr\_Topfield

Tigonus

Totambu

Tulune

Urvaius

Venom\_Masters

Wayrest

Wrothgaria

Ykalon

Zen

The first form of the Person command picks the gender of the NPC at random, while the later two forms request a specific gender.

anyInfo nn and rumors nn tags can be appended to supply message block numbers to use in the dialog screen.

    Person \_lamper\_ faction The\_Order\_of\_the\_Lamp anyInfo 1031

would select an NPC allied with The Order of the Lamp. Qrc message block 1031 is associated with the Tell me about.. dialog option of the game. The gender of the NPC is taken at random. (Although for the Order of the Lamp, females are rendered by male images as it happens)

* * *

### Quest Places

Different locations can be associated with a quest. For example, the PC may have to steal something from a shop, scour a dungeon for some special ingredient, or what have you. These places must be tracked both by the **X-engine** and the quest itself (You might want something special to happen when the PC arrives at a quest location)

A Place command is used to describe each place a PC might visit in the quest where something out of the ordinary may take place in pursuit of the quest goal.

Message blocks can be associated with locations in two different ways.

anyInfo nn adds the Qrc message nn to the Tell me about.. dialog responses.

rumors nn adds the Qrc message nn to the rumors to spread when the PC asks for _Any News?_ in the dialog panel.

These tags can be appended to any Place command. Although, beware that adding a remote dungeon to the pick list under the _Tell me about..._ dialog tab doesn't always do the right thing.

Instead of displaying the name of the dungeon in the pick list, **X-Engine** usually displays the name of a fictitious alchemy shop. However, rumors spread through the _Any News?_ query will display the remote dungeon name properly.

This unfortunate side-effect can be mitigated somewhat, by using the `[_dialog link_](#rumor5)` command to suppress the entry of the bogus shop name in the pick list.

Quest locations are divided into three groups:

*   permanent sites
*   local sites
*   remote sites.

* * *

### Permanent Sites

A permanent site is one that appears the same for every **Daggerfall** incarnation, and they are the obvious main story 'hot spots': Sentinel, Wayrest, Daggerfall, Dirinni Tower, Scourg Burrow, and so forth. Your quest is free to send the PC on a romp through Castle Daggerfall or Medora's tower if you want to.

    Place  \_symbol\_  permanent  aPermanentPlace

where aPermanentPlace is one of the permanent **Daggerfall** sites. Some places have several forms, each of which indicates a different location within the dungeon that the teleport cheat travels to. These are used to indicate specific rooms in the castle dungeons, or rooms along the chain of teleport cheats for other dungeons.

DaggerfallCity1

MantellanCrux

Sentinel4

DaggerfallCity2

OrsiniumCastle1

Shedungent1

DaggerfallCastle1

OrsiniumCastle2

Shedungent2

DaggerfallCastle2

OrsiniumCastle3

Shedungent4

DirenniTower

OrsiniumCastle4

Skeffingcoven

GlenmorilCoven

PirateerHold1

TotambuCoven

KykosCoven

PirateerHold2

WayrestCastle

LlugwychCastle

ScourgBarrow1

WoodbourneHall4

LysandusTomb1

ScourgBarrow2

WoodbourneHall5

LysandusTomb2

SentinelCastle



If you choose a permanent location, you may need to make it visible on the proper province map. The three main castles and the starting dungeon are always present on the province map. The dungeons for Lysandus' tomb, Nulfagu, etc are initially absent from the travel map.

The `[reveal](#revealglobal)` quest action can put permanent sites on the travel map.

Note also, that quest locations specified as permanent do not have their \_symbol\_ substituted properly in Qrc text blocks. So their names must be written out in full in any Qrc messages refering to that site.

* * *

### Local Sites

A quest can associate a house or shop in the town currently occupied by the PC--there are no local dungeons in **Daggerfall**.

    Place  \_symbol\_  local  aLocalSite

where aLocalSite is:

apothecary

house1

palace

armory

house2

pawnshop

bank

house3

random

bookstore

house4

tavern

clothingshop

jewelryshop

temple

furnitureshop

library

weaponstore

generalstore

magery



house1 through house4 select between different styles of **Daggerfall** homes. Random selects the type of shop at random when the quest begins.

* * *

### Remote Sites

A quest can associate a house or shop in a remote town, or it can associate a remote dungeon with the quest.

    Place  \_symbol\_  remote  aRemoteSite

where aRemoteSite is:

apothecary

generalstore

magery

armory

house1

palace

bank

house2

pawnshop

bookstore

house3

random

clothingshop

house4

tavern

dungeon

jewelryshop

temple

furnitureshop

library

weaponstore



When you specify a remote place, **X-engine** selects a location in the current province that has the type of remote site requested.

Note that this sometimes means the current location will be selected if it satisfies the site criteria. Really, _local_ implies that no distant travel will be required, while _remote_ implies that distant travel is optional.

Although there are no local dungeons, your quest scenario may request one. Why would you want to do this? One reason is to hide another quest resource where you are certain the PC can not locate it. Selecting a different remote dungeon that the quest does not reveal is not a certain guarantee that the PC can not locate the resource in the other dungeon. After all, the other dungeon may already be visible on the province map from an earlier quest. The present quest has no way of determining this fact.

* * *

### Quest Clocks

Quests can associate time limits or durations to various parts of the quest by creating clocks to time events. Clocks specify how much time must elapse before a task in the condition/response section is activated.

Clocks can specify an absolute amount of time, or a range of time, or a random amount of time. Use an extended version of clock time format to specify an amount of time:

    days.hours:minutes

so 1.4:45 represents a time 1 day, 4 hours and 45 minutes in the future.

    Clock  \_symbol\_

creates a clock with a random amount of time to run.

    Clock  \_symbol\_  dd.hh:mm

creates a clock that runs down in dd days, hh hours, and mm minutes.

    Clock  \_symbol\_  dd.hh:mm  aa.bb:cc

creates a clock that runs down between dd.hh:mm and aa.bb:cc.

The symbol assigned to a clock also names a task in the condition/ response section of the Qbn file. When the timer runs out, that task is actived in response to the time-out.

Note that a Clock resource doesn't automatically start ticking, and must be started by using the `[_start timer_](#starttimer)` action.

* * *

### Quest Enemies

Quests can create specific enemies to plague the PC with.

    Foe  \_symbol\_  is  anEnemy
    Foe  \_symbol\_  is  nn  anEnemy

The first form creates a single foe of type anEnemy while the second form creates nn foes of type anEnemy.

Please see the file foes.src for the complete list of quest foes.

    Foe  \_angryMages\_  17 mage

would describe a mob of seventeen (17) mages.

Unlike quest NPCs or places, quest foes must be installed in the game world through actions in the condition/response section of the quest file.
