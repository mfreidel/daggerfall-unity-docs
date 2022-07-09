# Quest Writing

## Using the quest decompiler

You can examine the contents of just about any **Daggerfall** quest. The only quests which are unavailable are those quests which have a Qbn file, but no Qrc file. Since there isn't any good way of figuring out what the missing text is, the decompiler won't show you the contents from such a 'mistake'. Some of the mismatches seem to arise from inconsistent reputation/serial numbers between qbn and qrc files.

The quest decompiler is fairly verbose, preferring plain English whenever feasible. Although it has its own ideas about capitalization, that is largely irrelevant as far as using the decompiled quest as the source to recompile a quest.

To decompile a quest, you need to to tell the decompiler which quest you're interested in, and where to put the disassembled quest information.

    template  -d  d:\\game\\dag\\arena2\\40c00y00.qbn  40c00y00.src

where:  
     template is the name of the quest decompiler.  
     \-d is the command switch specifying a quest decompilation.  
     d:\\game\\dag\\arena2\\40c00y00.qbn is the path to the quest of interest.  
     (Use your own **Daggerfall** installation path to the arena2 directory)  
     40c00y00.src is the file to received the decompiled quest.

If you omit this destination, the compiler will decompile the specified quest in the same directory it finds the qbn/qrc files and names the disassembled quest with the .src extension.

You can make changes to the quest by editing the .src file with the text editor of your choice. If you use a word processor be sure to insist that it leaves the text as pristine ascii (no format conversions please) if you want to recompile the quest.

* * *

Using the quest compiler
------------------------



To compile a quest, reverse the process:

    template  40c00y00.src

where  
     template is the name of the quest compiler.  
     40c00y00.src is the name of the quest to compile.

By default, the quest compiler creates the qbn/qrc files in the same directory it found the .src file in. The quest compiler will list the quest source on standard output, along with any complaints it has along the way. If the quest contains any errors at all, the last line of standard output will say so and advise you to review the output to correct the errors.

Please do not expect the _qbn/qrc_ quest files to be useable if the compiler finds errors.

You can make a verbose compilation listing by including the \-v switch:

    template -v 40c00y00.src

which will make the compiler recite everything it does to produce the qbn/qrc quest files, including its language construction from source patterns, as well as the binary code emited for the _qbn_ file.

In addition, the resulting quest scenario will contain the information required to use the **X-engine** quest debugger. See [Using the **X-Engine** quest debugger](#debugger) for more information.

* * *

Diagnostics
-----------



The compiler/decompiler will complain from time to time about anomalies when it encounters them. Some errors are simply reflected from the underlying operating system and won't be covered here. In such cases, the compiler generally annotates the system complaint with its own spin.

Error messages usually begin with the tag qcomp: although the Qrc compiler sometimes complains using the tag qrc: if it is unable to access the Qrc file for some reason.

The source line containing the error will usually be listed prior to the error message complaining about it.

* * *

NN is an invalid Qbn section number. Only \[0, 10\] are allowed.

The compiler discovered it was asked to create a non-existent Qbn section. It usually indicates Windows has shot itself in the foot or allowed some other application to shoot the compiler in the foot.

* * *

\_symbol\_ appears in Qrc without mention in Qbn.

The compiler detected a quest symbol that was used in a Qrc message block, but which was never defined by a corresponding quest resource. This usually means that either the symbol is misspelt in the Qrc block or in the quest resource, but it can't tell you which one is suppose to be right. Misspelt Qrc symbols will crash the quest, or at best give the infamous BLANK text.

* * *

\_symbol\_ at &N is the wrong type of symbol for this operation.  
Expected symbol type NN but found symbol type MM.

While reducing a qbn resource to its numeric form, the compiler discovered a usage which clashs with the type of operation being performed. For example

give pc \_house\_

can produce this error, since it isn't reasonable to install a quest location in the PC's inventory.

\_symbol\_ will be replaced by whatever resource the compiler is actually grieved about. NN and MM are Qbn section numbers.

* * *

\_symbol\_ has not been defined.

While reducing a Qbn resource to its numeric form, the compiler discovered it refers to a non-existant resource. \_symbol\_ is replaced by whatever text the compiler is actually grumbling about.

For example, defining an action to use an item resource without actually defining the item resource to be used can create this error condition. Mispelling an item's symbol can as well.

* * *

Bad DagText number NNNN (should be between 1000 and 2100).

An action in a quest task has specified a bogus Qrc text number. Bad DagText numbers can crash **X-engine**.

* * *

Buffer overrun.

While decompiling a Qrc file, the decompiler encountered a message block too large for its internal buffer area. The decompiler assumes every Qrc message block is smaller than about 1000 characters.

* * *

Cannot use a symbol for amount of gold reward.

An item gold directive tried to provide the amount of cash non-numerically. The compiler expects cash values to be numbers.

* * *

Cannot use same symbol with differing contexts.  
Previous context is:

The compiler encountered an invalid symbol usage. The previous context is the resource definition which conflicts with the present resource definition. For example

    Item \_reward\_ gold 10  
    \_reward\_ task:

would trigger a conflict because all item and task names must be unique.

* * *

Code parameter count must be in \[1, 5\].

The compiler discovered a Qbn template in its .src database with a suspicious number of operands defined. All known Qbn condition/responses take between 1 and 5 operands.

* * *

Could not convert the new size value.

A .src file containing a size: NN directive failed to convert NN without error.

* * *

Could not open aFileName for input.

The compiler was unable to access the file aFileName for input.

* * *

Could not open 'aFileName' for input.

The compiler/decompiler was unable to access the .src/.qbn file to operate on.

* * *

Hmmm, this quest doesn't seem to have anything to say.  
('qcomp' would like some text to work with)

The compiler discovered that it would create an empty Qrc file, which doesn't seem very likely.

* * *

Implicit task gives template more than NN words.

A resource directive in the .src database has tried to install more arguments in a resource brick than are expected.

* * *

Line NNNN 'fragment'

At line NNNN of the quest source file, the compiler stumbled while analysing a fragment of that line. A complaint detailing the trouble it got into generally follows.

* * *

No command handler for Qbn section NN has been created yet.

The compiler discovered it was asked to create an unused Qbn section. Since all the known Qbn sections in use have handlers created for them, in practice this should not arise unless Windows is taking pot-shots again.

* * *

No command number; cannot convert template to Qbn section.

A command/directive template has been added to the compiler's .src database that does not properly define a Qbn resource. All well-defined resource directives must specify an index to the Qbn section as the first field of the template defining the resource.

* * *

No template matches this pattern.

The compiler could not locate a match for a resource directive in its .src data base.

* * *

Quest contains errors. Please review the output log.

One or more errors occurred during the processing of the quest source file. The Qrc/Qbn files for this quest will in general be unusable.

* * *

Quest: 'aFilePattern' doesn't follow the pattern for **Daggerfall** quest file names.

The Quest: directive used in the quest preamble does not follow the pattern for **Daggerfall** quest file names.

* * *

Seek to text offset barfed (NN != MM).

While positioning the Qrc file to process an existing message block, the decompiler discovered that the position requested did not match the position arrived at. This generally indicates someone else is trying to update the Qrc file at the same time the decompiler is trying to read it.

* * *

Source file has no extension.

While trying to infer the Qrc/Qbn file name from the source file name, the compiler discovered that no .src extension was present. For the nonce, the compiler relies on the .src extension for various internal reasons.

* * *

Stopping compilation because Qrc compile has failed.

Quest compilation proceeds in two parts. First the message blocks are assembled to produce a Qrc file. Then the quest resource definitions are assembled to produce a Qbn file. Each part will generally try to compile all of the statements associated with it so you can receive all the compiler's complaints en masse, so to speak. However, if the Qrc part did not compile successfully, the Qbn part notices this fact and doesn't start.

* * *

Template failed to assign an item.

One of the object templates has failed to assign a required parameter for that type of object. If you modify one of the compiler .src files (to define additional items perhaps) that change has offended the compiler somehow.

* * *

This clock doesn't have an associated timer task.

The compiler tried to assemble an action refering to a clock resource and discovered that no task had been created for that clock. **X-engine** expects every timer to have a task defined for it, even if there are no actions to associate with the task.

* * *

Too many Daggerfall variables. Symbol table size of NN is too small.

While compiling the Qrc message blocks, the compiler discovered the table it uses to track misspelt symbols was used up. This table can hold up to 1000 symbols, which ought to be adequate for ordinary quests.

* * *

Unable to create command output.

The compiler was unable to manufacture an argument brick for a quest resource. This usually indicates that system resources or memory are low.

* * *

Unable to determine the <parameter> (NN).

Quest resources (like NPCs, Foes, etc) have to be reduced to a numeric form for the **X-engine**. When the compiler tried to do so, it couldn't figure out how to represent the parameter as a number. This usually means you've mispelled a quest resource or used a quest resource that isn't present in the compiler's .src database. If you've made changes to the compiler's .src database, the field number where the mishap occured is given by NN. parameter spans the names of the qbn fields for all the Qbn record types.

* * *

Unable to determine the permanent site.

A quest Place resource has requested an unknown location as a permanent site. Either it didn't recognize the spelling for the permanent site, or the requested site isn't part of the permanent site database.

* * *

Unable to open aPathName for reading.

While constructing the quest resource database, the compiler encountered a .src file name which it could not access.

* * *

Unable to open qrc file aFileName.

While creating a new quest, the compiler was unable to access the Qrc file for writing.

* * *

Unable to open the decompiled file 'aFileName'.

While setting up to disassemble a quest, the decompiler was unable to access the source file aFileName when it tried to create it or reuse it.

* * *

Unable to open the qbn file 'aFileName'.

While setting up to disassemble a quest, the decompiler was unable to access the Qbn file aFileName.

* * *

Unable to position the qrc file to next message descriptor.

While decompiling an existing Qrc file, the decompiler was unable to scroll the file to the next message block that it expected to process. Unless you're having hard drive problems, this usually indicates a mutant Qrc file has been found.

* * *

Unable to rewind the qrc file.

While decompiling a quest, the decompiler discovered it could not reposition the Qrc file to its beginning.

* * *

Unable to write an empty qrc file header.

While creating a new quest, the compiler was unable to write the Qrc file preamble.

* * *

Unknown configuration directive.

While processing its configuration file (_template.cfg_) the compiler encountered an unknown configuration directive.

* * *

Unknown switch '-x'.

While analyzing the command line arguments, the compiler encountered an unrecognized switch or setting.

* * *

Unable to open qrc file aFileName.

While setting up to disassemble an existing quest, the decompiler discovered it was unable to open the Qrc file of the Qrc/Qbn pair.

* * *

Unresolved symbolic references.

The compiler was presented with a quest resource definition which did not completely replace all symbols with numeric values in the resource argument brick. There should be one or more preceding complaints detailing which bits could not be resolved.

* * *

Warning: &N did not resolve to a numeric value.

After performing all the symbolic resolutions correctly, the compiler detected that parameter N was still a non-numeric value. This usually means that one of the .src files in the compiler's data base has been edited to create a non-numeric template. Typing the letter O instead of the number 0 can cause this.

* * *

Warning: making system quest file.

The compiler discovered that the file name pattern for the quest indicates that a main-story quest is being produced. The interrelation between main-story quests and their global states isn't well-understood, so the risk of substantial changes having surprising side-effects is higher in these cases.

* * *

Warning: no command templates have been detected.

After processing its configuration file (template.cfg) the compiler detected that no quest resource operations have been included.

* * *

Warning: no item definitions have been detected.

After processing its configuration file (template.cfg) the compiler detected that no quest items were present in the resulting database.

* * *

Warning: template does not assign all parameter tokens.

Each quest resource is determined by an argument brick of parameter values required by the **X-engine** to describe that resource. The compiler discovered a command template that failed to provide values for all of the parameters required to describe the resource.

* * *

Warning: this configuration has no command template files.

After processing its configuration file (_template.cfg_) the compiler detected that no quest resource operations have been included.

* * *

Well, that didn't work.

After processing a configuration directive the compiler noticed that something was amiss. Generally there is a preceding complaint detailing the trouble.

* * *

Wrong number of command line arguments.

While processing the command line arguments, too many parameters were encountered.

* * *

Using the **X-Engine** quest debugger
-------------------------------------



When creating new quests, it is helpful from time to time to see which steps of a quest have been taken, and which have not.

**X-Engine** has a cheat mode key to overlay the main view with the tasks of a quest to show which ones have been acted on.

To access the **X-Engine** debugger, edit z.cfg and include the line

    cheatmode 1

in the file before starting **Daggerfall**.

After acquiring the quest you want to examine, you can use the apostrophe or single quote key (') in the main 3D view to overlay a display of the quest tasks on the main view, on an active quest by active quest basis.

When the overlay is active, the name of the quest, along with the player position, is displayed at the bottom of the overlay. You may need to adjust the camera position to find a suitable contrast between the view background colors and the yellow letters of the overlay.

The apostrophe or single quote key (') will step the display through each active quest of the character. Each time the key is pressed, the tasks of the next active quest of the PC will be displayed, until the display finally returns to the first quest shown.

The quests are displayed in no particular order, so you will have to cycle through the open quests until the one you're interested in appears on the screen.

To clear the task overlay from the screen, press **F9**. If you have used the game's control panel to modify the behavior attached to **F9**, you may need to revert that assignment to enable **F9** to clear the overlay display.

For each open quest, **X-Engine** displays a flag to indicate which steps of the quest have been taken, and which are as yet inactive.

Tasks are listed in the same order which the scenario declared them. But as a fail-safe check, the symbol name of each task is displayed in numerical form. Executed tasks are highlighted in dirty orange; unexecuted ones in yellow.

When you use the _\-v\[erbose\]_ option of the compiler program, the end of that listing will show the same numerical values which will appear in the **X-Engine** debug screen.

In addition, debugging information is added to the quest file which **X-engine** will access to provide symbols in the display overlay in place of raw numbers.

When _\-v_ is used to compile a quest, after cycling the display to that quest with the apostrophe (') key, you can use the semicolon (;) key to cycle through the various quest resource sections.

For _items_, _persons_, and _foes_, the world coordinates of the thing are displayed. If the thing hasn't yet been installed in the game world, its coordinates will be zero. If the thing is in the same vicinity as the PC (the current town or dungeon) it will be highlighted a dirty orange color, else displayed in yellow.

For _clocks_, the elapsed time and the alarm time are displayed in game minutes.

For randomly selected _dungeons_, whether the location has been revealed or not is available, as well as its world coordinates once it has been revealed. Note that even if the dungeon has already been revealed on the province map by an earlier quest, this display shows whether the present quest has revealed it or not. Other quest locations are never hidden, and so always appear on the world map.

And as before, without the debugging information, the state of the tasks of the quest is available, but now all the tasks have their names from the quest scenario source displayed too. Executed tasks are displayed in dirty orange, unexecuted ones in yellow.

Note that the compiler calls these _symbols_, while **X-Engine** calls them _flags_\--they refer to the same task block in the quest scenario.

When **X-Engine** indicates a flag is _true_, this means that task is presently executing/has already executed. When **X-Engine** indicates a flag is _false_, this means that task has not been executed, or that the task has already been cleared so it may rerun when conditions are ripe.

Certain game conditions (e.g., _daily from_) are automatically rescheduled for execution each game world minute, and so always are reported as _false_, even though the task has executed.

Again, use the apostrophe (') or single quote key to cycle through the different open quests. Use the **F9** to remove the task overlay from the screen.

Examining living quests
-----------------------



The quest decompiler engine can be used to examine the contents of an open quest in the player's logbook. In addition to the usual disassembled details, links to other saved game records for items, NPCs, and places are available, among other details. The native Linux decompiler supports date recovery, which is not available without the ATLAS package and isn't implemented under Dos.

To decompile a living quest, the desired quest must first be extracted from the saved game of interest. The TES2 tool will extract all the open quests with its _\-Q_ option.

    C> tes2 -Q 3

extracts all the living quests in saved game slot 3 into the fictitious quest player.qbn. Each open quest is assigned an arbitrary serial number (1, 2, 3, ...) when the saved game is created. The serial numbers change whenever quests start up or stand down, and so must be determined _in situ_ each time. The end of the regular TES2 report shows the serial numbers currently in use by open quests:

                           Active Quests
       (2)         s0000977                  (4)         S0000021             
       (6)         s0000012                  (3)         s0000999             

The gaps (2, 3, 4, 6) in the serial numbers are normal.

To examine the living state of quest 3, s0000999, use the SAVEQBN tool to extract quest 3 from player.qbn and feed the extracted quest to the decompiler using the _\-s_ switch instead of the usual _\-d_ switch:

    C> saveqbn 3 <player.qbn >s0000999.qbn
    C> template -s s0000999.qbn snapshot.src

where _\-s_ stands for _sans_ Qrc. For example, from an unrelated living quezt

--	Quest start-up:
	place item \_I.01\_ at \_L.04\_
--	exec: 1 (000b58c6) 09:42 Morndas, 7 of Mid Year, 405.
--	hash: 00644605 44ebfa04 00000000 00000000
	dialog link for person \_victim\_
--	exec: 1 (000b58c6) 09:42 Morndas, 7 of Mid Year, 405.
--	hash: 00000000 483b3e45 00000000 00000000

The _exec_ tag gives the last time of access and whether the action has been accomplished yet. _0_ generally means _not performed_ while _1_ indicates _completed_. Other states are possible, e.g. for letters as yet undelivered to the PC but already _en route_.

The _hash_ tag lists the object identifiers for the four possible opcode operands. There will be an item record in the saved game with object id 0x00644605, a quest location with object id 0x44ebfa04, and an NPC record with id 0x483b3e45.

NPC and place object identifiers use the Daggerfall GPS notation. So this quest NPC's birthplace is somewhere in site 0x483b. ATLAS shows that 0x483b is Langpath, Anticlere. The specific lot number, 0x3e45, is tied to a quest location 0xfann by another record in the saved game. If the PC isn't presently in Langpath, that relation won't necessarily exist until the Langpath site has been rendered.

MAPS.BSA identifies the possible quest locations at a site with a special codex listing the lot numbers that correspond to quest locations. A quest site, whether in town or in a dungeon, is indexed by the special lot code 0xfann, where nn is the consecutive serial number of quest locations at a site (0xfa01, 0xfa02, ...). If the site is a dungeon, these correspond to the rooms which the teleport jump cheat links together.

The place 0x44eb is the Ruins of Ashsly Manor in Anticlere province, where the fourth teleport jump has been randomly assigned as the spot where the quest item will be hidden.

You can use the _\-s_ option to decompile just the **Qbn** portion of an ordinary quest file, but the resulting extra information is uniformly uninteresting.


## Quest Organization

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

Note that certain quest actions will post this message automatically. See [_give pc_](#givepc) / [_give pc nothing_](#givepc3).

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

**X-engine** likes to name replaceable parts of text as \_symbol\_, where in fact symbol can be any meaningful mnemonic. (See [Quest Resources](#qbn) for how symbols are created)

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

\[1\] Reproduced from Appendix A, _Daggerfall Quest File (QRC/QBN) Hacking Results - v1.00_, by Dave Humphrey [\[email protected\]](/cdn-cgi/l/email-protection) I've added 2 or 3 additional symbols from the qrc files.  
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

But sometimes a scenario wants to make such information available only after the PC has met certain milestones within the quest. In such cases, the scenario can control when the _anyInfo_ messages become available through the [_dialog link_](#rumor5) and [_add dialog_](#rumor1) family of quest actions.

Note that once an entry has been added to the _Tell me about..._ pick list, it is stuck there for the remaining life of the quest. There does not appear to be any quest action to delete entries from the list.

A person or place quest resource may be further embellished with the tag

    rumors aQrcNumber

where aQrcNumber is a Qrc message block to display in the dialog screen when the PC selects the _Any News?_ dialog option.

Oftentimes, adding a _rumors_ tag will insert the quest resource into the dialog picklist as well, but this insertion can be suppressed with [_dialog link_](#rumor5) family of commands.

* * *

### Quest Items

Every item associated with the quest must be described for the **X-engine** using a form of the Item command. Items include not only things, like rings, letters, drugs, or artifacts, but also quest rewards like gold payment.

In addition each item can have message text from the Qrc file associated with it, which is displayed when the item is discussed with an NPC or when the PC 'uses' the item in the inventory screen.

Quest items with usage text will also appear in the About picklist when conversing with NPCs. The Qrc text will be 'spoken' by the NPC if asked about the item.

Whether the item automatically appears as a topic under the _Tell me about..._ dialog tab is somewhat variable. Non-letter items tend always to appear, while letter items tend to depend upon their method of delivery to the PC.

Regardless, the scenario can regulate the contents of the _Tell me about..._ picklist with the [_dialog link_](#rumor5) and [_add dialog_](#rumor1) actions.

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

The quest condition [_faction available_](#factionfree) can help to determine whether a permanent NPC is presently starring in another quest.

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



By coordinating the occupation of the NPC with site selection, you can control the venue that your NPCs appear in. Selecting a spellcaster as an NPC and a mage's guild hall for a site ensures the resulting sprite will be one the player is accustomed to finding in that locale. See [_Local and Remote Sites_](#localsites) for additional information.

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

This unfortunate side-effect can be mitigated somewhat, by using the [_dialog link_](#rumor5) command to suppress the entry of the bogus shop name in the pick list.

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

The [reveal](#revealglobal) quest action can put permanent sites on the travel map.

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

Note that a Clock resource doesn't automatically start ticking, and must be started by using the [_start timer_](#starttimer) action.

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

* * *

Quest Condition/Responses
-------------------------

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

Note also that each Clock command creates an implicit time-out condition. Create a task with the same name as the clock to activate when the clock runs out. Note that a Clock resource doesn't automatically start ticking, and must be started by using the [_start timer_](#starttimer) action.

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

Using a combination of _ands_, _ors_, and _nots_ it is possible to construct a contingency from any of the preceding basic conditions, which can make for very interesting plot lines, especially when combined with the [random task selection](#pickone) command.

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

This condition checks whether the specified faction is available for assignment as a questor in an upcoming quest that the present quest would like to schedule. nnn is either a faction number, or the proper name of a faction. See the [_Person_](#persons) command for the complete list of factions.

This condition is triggered by the player clicking the mouse on an NPC sprite associated with the specific faction in the game world.

The main quest monitors the availability of NPCs connected with the main story before attempting to schedule a new subplot. The player triggers this check by clicking on the NPC sprite that corresponds to the specified faction.

Some permanent NPCs play a role in several different subplots. To avoid inadvertently scheduling a new subplot which uses a permanent NPC in a starring role while the same NPC is already starring in an active quest, the monitor quest checks whether the desired NPC is free at the moment, to schedule their entrance in the upcoming subplot.

* * *

Quest Actions
-------------

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

To tie chatting wih townspeople about current events to a quest, you can add stock dialog replies about quest npcs, items, and places to the replies given to the _Any news?_ query in the dialog screen. See [dialog link](#rumore5) for details.

You can also add additional entries to the picklist under the _Tell me about_ dialog tab. Note that there are two ways to incorporate quest-related information into the dialog screen: the incredibly obvious, and the not so obvious.

When you create a quest [Item](#items), [Person](#persons), or [Place](#questplaces) adorned with a Qrc text number, this creates an incredibly obvious dialog opportunity, by adding the thing in question to the principle pick-list under the _Tell me about_ dialog tab.

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

If the item has been linked to another quest resource by the [_dialog link_](#rumor5) command, when the NPC recites the Qrc message from the item's _anyInfo_ tag, the linked quest resource name is added to the dialog pick list, just as though the quest scenario issued an appropriate _add dialog_ command for the linked resource.

Please note, if you want to use linked dialog entries in a quest, you must first use the [_dialog link_](#rumor5) command to establish the desired relationship before using the _add dialog_ command to update the dialog picklist.

    add dialog for person anNPC

This action adds _anNPC_ to the _Tell me about_ dialog picklist and includes the Qrc text block from the person's _anyInfo_ tag. If the person has no _anyInfo_ tag, no entry for the person will be created.

If the person has been linked to another quest resource by the [_dialog link_](#rumor5) command, when the NPC recites the Qrc message from the person's _anyInfo_ tag, the linked quest resource name is added to the dialog pick list, just as though the quest scenario issued an appropriate _add dialog_ command for the linked resource.

Please note, if you want use linked dialog entries in a quest, you must first use the [_dialog link_](#rumor5) command to establish the desired relationship before using the _add dialog_ command to update the dialog picklist.

    add dialog for location aSite

This action adds _aSite_ to the _Tell me about_ dialog picklist and includes the Qrc text block from the place's _anyInfo_ tag. If the place has no _anyInfo_ tag, no entry for the place will be created.

If the location has been linked to another quest resource by the [_dialog link_](#rumor5) command, when the NPC recites the Qrc message from the place's _anyInfo_ tag, the linked quest resource name is added to the dialog pick list, just as though the quest scenario issued an appropriate _add dialog_ command for the linked resource.

Please note, if you want use linked dialog entries in a quest, you must first use the [_dialog link_](#rumor5) command to establish the desired relationship before using the _add dialog_ command to update the dialog picklist.

If you have several different kinds of quest resources to gossip about, you can combine location, person, and item (in that order) with one add dialog for command:

    add dialog for location \_store\_ person \_contact\_ item \_gem\_
    add dialog for location \_house\_ item \_letter\_
    add dialog for person \_qgiver\_ item \_ring\_

* * *

### Magically enhancing a foe

    cast aSpell on aFoe

You can magically enhance any foe in a quest by casting one of the standard spells with this action. Of course, you can repeat the action any number of times with different spells to achieve whatever effect you would like to have.

Please see the [_spell detection_](#spellscast) condition for the spell enhancements which are available. For example

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

To create foes in a more guarded manner, see [placing foes in the game world.](#placefoe)

The spoils from the dead bodies of human foes generated by this command have the interesting properties of (a) improving in quality with each wave and (b) repeating the spoils dropped by the foes dispatched in the previous waves. The spoils are also proportional to player level, so higher level PCs tend to see better goods. But even at level 2 or 3, mithril drops aren't that uncommon.

I've had PCs garner over 5 million gold from such an episode. Even at level 2 or 3, gaining enough gold to purchase a yacht is commonplace.

The enemies spawned by _create foe_ will track the PC, both in town and inside dungeons. Some quests prefer to limit the appearance of spawned mobs to the town areas of the map. See [_send foe_](#sendfoe).

Note that the [_injured_](#attackingfoes) condition can be triggered for each wave of attackers, while the [_killed_](#attackingfoes) condition will only trigger up to the number of foes determined from the Foe command, no matter how many of them are actually slaughtered by the PC.

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

See the [place NPC](#placenpc) command for the usual way of positioning an NPC in the game world.

In general, permanent NPCs use the create npc action prior to using the place npc action. A permanent NPC used only with the place npc action isn't guaranteed to be visible within the game world.

* * *

### Curing diseases

    cure aDisease

The PC can be afflicted by disease, either during a quest (see [make pc ill](#ill)) or through exposure to various elements (foes with poisoned blades, were-things, etc).

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

This action completely obliterates a quest _Person_ resource. This is different from [_hide npc_](#removenpc) in that with the latter form, the resource symbol is still available for substitution within Qrc messages blocks.

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

When you create a quest [Item](#items), [Person](#persons), or [Place](#questplaces) adorned with a Qrc text number, this creates an incredibly obvious dialog opportunity, by adding the thing in question to the principle pick-list under the _Tell me about_ dialog tab.

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

In addition to the preceding discussion about rumors, when the time is ripe, the quest can expliciting insert a dialog topic for _\_theRelic\__ into the dialog picklist by using the [add dialog](#rumor1) command:

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

It is important to remember to use the [add face](#addface) command at some point during the quest before using drop face so the usage counts for the NPC's image will be intact when the quest ends.

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

These actions bestow a quest-related item directly into the PC's inventory without notifying the player of the transaction. That doesn't mean a previous or subsequent action can't inform the PC about the event, just that the player isn't required to manipulate an item using the inventory screen to pick up a quest item from (See [give item](#givepc) to bestow an item through the inventory picklist)

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

To remove a quest item after its initial bestowal, see [take item](#moveto).

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

This action removes the specified quest NPC sprite from the game world. See [restore npc](#clickable) action to replace the NPC sprite.

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

See the [remove log step](#changeglobal2) action to obliterate a log book entry.

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

This action installs a quest foe or mob of foes at a quest location. Quest-related foes won't appear in a quest until this action is taken to position them within the game world. (But see [create foe](#createfoe))

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

This action removes an entry inserted in the player's log book by a previous [log step](#log) action.

* * *

### Controlling NPC sprites

    restore anNPC

This action restores the indicated NPC sprite in the game world. Typically it follows a preceding [hide npc](#removenpc) task to make the NPC invisible, but still present so any Qrc blocks referring to the NPC still behave properly, but the PC will be unable to locate the NPC physically.

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

The _send foe_ command operates like the [_create foe_](#createfoe) command, but limits the spawning of _aFoe_ to the times when the player is above ground, generally near towns.

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

This action removes a previously created quest item (see the [get/give](#getitem) series of actions or [place anItem](#placeitem) to create a quest-related item) from the player's inventory, and from the game world.

* * *

### Teleporting the PC

    teleport pc to aPlace
    transfer pc inside aPlace  province#  magicNumber

You can relocate the PC to another position in the current province by using the teleport pc form of the command. Existing quests use this to relocate the PC to a remote dungeon trap in the same province, as though the teleport spell anchor were placed there.

It isn't known whether this form of the command will teleport the PC to permanent sites.

The alternate form, transfer pc, is used at the end of the main story when Nulfaga teleports the PC to the final dungeon of the Mantellan Crux. It is also used to return the PC to Nulfaga when the Crux quest is completed.

province# and magicNumber are determined as for the [reveal command.](#revealglobal)

* * *

Common pitfalls
---------------

There are a number of pitfalls to using the X-engine for quests. This section documents known issues with the quest engine, and tries to explain them in a general way. Broadly stated, X-engine cannot reliably handle simultaneous events, and imposes a serial or chronological order that the quest author does not realize.

Technically, these are sometimes called _race conditions_, because, like the winner of a foot race, the task which executes first cannot be predicted by the quest author.

### Unable to check repute

Using the [when repute with _aFaction_](#factionrepute) condition to examine the PC's standing with a Daggerfall faction can fail spontaneously when the result of that task is examined in a subsequent [when condition](#multipletests).

    \_reputeOK\_ task:
        when repute with Cyndassa is at least 17

    \_clickedCyndi\_task:
        clicked \_cyndi\_

    \_nextStep\_ task:
        when \_clickedCyndi\_ and not \_reputeOK\_

    \_alternateStep\_task:
        when \_clickedCyndi\_ and \_reputeOK\_

In this quest fragment, it is unpredictable whether \_nextStep\_ or \_alternateStep\_ is performed, regardless of the PC's repute with Cyndassa. \_nextStep\_ may always be performed, and if the PC's repute is high enough, both tasks will execute, which is almost certainly not what you expect to happen.

There is a race between \_reputeOK\_ and \_nextStep\_. The order of the tasks in the scenario seems to imply the \_reputeOK\_ task is considered before the \_nextStep\_ task, but in fact, X-engine may check the \_nextStep\_ condition _before_ it checks the \_reputeOK\_ condition.

The race condition can be partly avoided by rewriting the \_reputeOK\_ task:

    \_reputeOK\_ task:
        when repute with Cyndassa is at least 17
        clear \_nextStep\_

    \_clickedCyndi\_task:
        clicked \_cyndi\_

    \_nextStep\_ task:
        when \_clickedCyndi\_ and not \_reputeOK\_

    \_alternateStep\_task:
        when \_clickedCyndi\_ and \_reputeOK\_

This rewrite won't work right if either \_nextStep\_ or \_alternateStep\_ have _any_ actions associated with them.

Sometimes it's possible to rearrange the steps of a quest to accommodate this race condition. Othertimes it is not. It may be helpful to recall that every task starts out unexecuted, and so some condition which is _not_ is always true to begin with.

* * *

### Unable to repeat messages

How X-engine handles quest dialog is counterintuitive in a number of ways. The basic rule is that _any quest condition/response that posts a Qrc message on the screen will be performed no more than once, no matter how many times the conditions for its performance are satisfied._

That is, the user will see the message text only the first time such a task is performed, no matter how many subsequent times the task might be reasonably performed.

This seems harmless enough, until you want a quest to post some message in response to clicking on a quest NPC, perhaps to repeat some mission critical quest information. Despite the propaganda in the preceding sections, the X-engine will not provide such service.

Once a condition/response has posted a Qrc message, it will never reshow that message. Period.

This seems to be genuine oversight in the quest engine design. If your quest requires a Qrc message to be repeated in the condition/response section, the only way to make that happen is to arrange for seperate tasks to display the message each time you want the message posted.

This is a fairly severe limit on what a quest can and cannot do.

A common desire is for a quest NPC to repeat a speech as often as the player clicks the mouse on that NPC. X-engine simply does not allow this idiom to be used.

* * *

### Unable to stop sounds

Unlike message posts, a sound once started by the [play sound](#playsound) command performs indefinitely.

To stop a playing sound, clear the task that started it.

    Clock \_silence\_

    \_makeNoise\_ task:
        play sound halt every 2 minutes 3 times

    \_silence\_ task:
        clear \_makeNoise\_

In this quest fragment, when the \_silence\_ timer runs down, the noise maker task is stopped.

* * *

Reusing Quest elements
----------------------



There are three additional compiler commands to help you organize the information in a quest for use in new quests you decide to create.

* * *

The cfg: directive allows you to modify the compiler's notion of what quest elements are available by adding your own item and quest commands to its description of the **Daggerfall** environment.

### cfg: aFileName

where aFileName is the name of the configuration file describing your additions.

Your changes will be available for the remainder of compile task.

There is no specific way to revoke your changes until the compile task completes. Once the compiler finishes it forgets about your changes, and so it has to be told about them each time you want to compile a quest that uses those alterations.

* * *

The src: directive allows you to include another quest file verbatim at the place where the src: directive appears.

### src: aFileName  
<src: aFileName\>

where aFileName is the name of the quest file that you want to insert into the present quest. Note that the named file doesn't have to be a complete quest. It might be consist of some stock dialog you'd like to include in several quests. Or it might be a series of condition-responses that you'd like to reuse in several different quests.

Use the first form to include quest resources, tasks, and complete Qrc message blocks.

Use the second form to include text within a single Qrc message block. This allows a single set of messages to be reused with different text blocks.

    RumoresDuringQuest:
    <src: ../src/harth.rum>

    RumoresPostfailure:
    <src: ../src/harth.rum>

    RumoresPostSuccess:
    <src: ../src/harth.rum>


would share the same set of rumors during the quest and regardless of the quest's outcome.

The compiler treats the included text verbatim, just as though you'd typed the file's contents in by hand at that very spot in the quest file. So, the quest elements which are included must make sense in the context where the src: directive appears.

For example, don't expect the compiler to be thrilled if you try to include some Qrc text blocks in the middle of the Qbn section.

* * *

### Substitutions

It's possible to combine the effects of configuration changes with source inclusion.

For example, you may have a series of condition/responses that you'd like to reuse in several different quests. But you don't want to be limited to using the same quest symbols each time.

It's possible to write those quest tasks just once, then tell the compiler how to use them with different items, npcs, and so forth, by writing a _macro_\--which is just a technical word for substituting one thing for another. Since the result of substitution is usually something _bigger_, the Greek root has stuck with us.

The compiler recognizes the beginning of a macro by the special command macro. And it recognizes the end of a macro by the special command end macro. All the commands between the two are eligible for special substitutions when those commands are actually used.

### macro aCommandPattern

aCommandPattern tells the compiler how you'd like to refer to the collection of commands between the macro command and the end macro command.

    Macro MeetBigP &1 trap &2 foes &3 &4 &5 &6
    :
    :
    End macro
    :
    :
    MeetBigP \_kahuna\_ trap \_dungeon\_ foes \_spider\_ \_bat\_ \_rat\_ \_gargoyle\_

The macro command tells the compiler how to recognize the new MeetBigP command. When the compiler encounters a subsequent MeetBigP command, it will substitute \_kahuna\_ for each occurance of &1 within the macro; \_dungeon\_ for each occurance of &2; \_spider\_ for each occurance of &3; and so forth.

Suppose the MeetBigP macro looks like this

macro MeetBigP &1 trap &2 foes &3 &4 &5 &6
atP task:
	clicked &1
	repute with &1 exceeds 34 do \_happyP\_
	repute with &1 exceeds 22 do \_smugP\_
	repute with &1 exceeds 10 do \_snideP\_
	say PopSteamed
	change repute with &1 by -4
	teleport pc to &2

\_snideP\_ task:
	say PopPeeved
	change repute with &1 by -2

\_smugP\_ task:
	say PopOk
	change repute with &1 by +2

\_happyP\_ task:
	say PopNice
	change repute with &1 by +4

\_battle1\_ task:
	when \_smugP\_
	create &3 every 180 minutes 100 times with 80% success

\_battle2\_ task:
	when \_snideP\_
	create &4 every 165 minutes 100 times with 65% success
	create &5 every 180 minutes 100 times with 65% success
	create &6 every 191 minutes 100 times with 65% success

end macro

When the compiler encounters the actual use of MeetBigP it notices the matching parts of the definition and replaces the MeetBigP command with the substituted macro lines, as though they had been typed verbatim at that place.

atP task:
	clicked \_kahuna\_
	repute with \_kahuna\_ exceeds 34 do \_happyP\_
	repute with \_kahuna\_ exceeds 22 do \_smugP\_
	repute with \_kahuna\_ exceeds 10 do \_snideP\_
	say PopSteamed
	change repute with \_kahuna\_ by -4
	teleport pc to \_dungeon\_

\_snideP\_ task:
	say PopPeeved
	change repute with \_kahuna\_ by -2

\_smugP\_ task:
	say PopOk
	change repute with \_kahuna\_ by +2

\_happyP\_ task:
	say PopNice
	change repute with \_kahuna\_ by +4

\_battle1\_ task:
	when \_smugP\_
	create \_spider\_ every 180 minutes 100 times with 80% success

\_battle2\_ task:
	when \_snideP\_
	create \_bat\_ every 165 minutes 100 times with 65% success
	create \_rat\_ every 180 minutes 100 times with 65% success
	create \_gargoyle\_ every 191 minutes 100 times with 65% success

If another quest had different NPC, say \_queen\_, with whom the same sort of behavior is needed, but perhaps with nastier foes, the MeetBigP command can be used with different elements:

    MeetBigP \_queen\_ trap \_remoteTown\_ foes \_nymph\_ \_vampire\_ \_knight\_ \_lich\_

which would result in the same set of actions, but tailored with a different NPC and foes.

Any part of a macro can be tailored. You aren't particularly limited to NPCs or foes. Instead of varying the types of foes, the command could have chosen to vary the frequency of attacks for example.

However, you cannot write a macro which substitutes a command name at all. Replacing all the _repute_ commands in MeetBigP with &7 won't let you change that command on the fly each time the MeetBigP macro is used.

* * *

Sample quests
-------------

### Lady Brisienna

Lady Brisienna starts the player character off on the first quest when the game begins. The decompiler renders her quest as follows.

-- Quest: /dos/g/game/dag/arena2/\_brisien.qbn.
-- StartsBy: letter
-- Questee: anyone
-- Repute: 0
-- QuestId: 0
Quest: \_brisien
-- Message panels
QRC:

RefuseQuest:  \[1001\]
Error: Questtext 1001 called on BRISIEN.Q

AcceptQuest:  \[1002\]
Error: Questtext 1002 called on BRISIEN.Q

QuestFail:  \[1003\]
Error: Questtext 1003 called on BRISIEN.Q

RumorsDuringQuest:  \[1005\]
The prophets say that soon what was unclaimed will be claimed.
<--->
Lady Brisienna awaits someone at \_dirtypit\_ in \_\_dirtypit\_.
<--->
That was some storm. Magically created, many are saying.
<--->
Lady Brisienna has taken up lodging at \_dirtypit\_ in \_\_dirtypit\_.

RumorsPostfailure:  \[1006\]
Lady Brisienna has left \_dirtypit\_, muttering about traitors to the Empire.

RumorsPostsuccess:  \[1007\]
Lady Brisienna has left \_dirtypit\_. She must've found who she was looking for.

QuestorPostsuccess:  \[1008\]
<ce>        I hope your investigations are going well so far, %pcf.
<ce>                                    
<ce>                               -- Lady B.

QuestLogEntry:  \[1010\]
%qdt:
 I am on a mission from the emperor to investigate
 the shade of King Lysandus. His spirit has been
 haunting the city of Daggerfall. The emperor
 himself has charged me with the duty of laying
 his ghost to rest.
    There is also the minor matter of a letter
 he sent to the Queen of Daggerfall. If I should
 find out what happened to the letter, he would be
 most appreciative.
    Before landing in Daggerfall, a sudden storm
 capsized the ship. I barely made it into this cave.

Message:  1011
%qdt:
 I met with Lady Brisienna in a tavern room. She
 told me that the three major powers of the Iliac
 bay are Daggerfall, Wayrest, and Sentinel. If I
 am to investigate the mystery of Lysandus' ghost,
 I should start by speaking with the royal families
 of these fiefdoms.

Message:  1012
Lady Magnessen? She's the emmisary of the Blades in Daggerfall.
<--->
Lady Magnessen is a part of the set regularly at Castle Daggerfall.
<--->
Lady Magnessen was formerly in the employ of Mynisera, when she was Queen.

Message:  1013
Lady Magnessen once worked for the Queen Mother, but always for the Emperor.
<--->
Lady Magnessen is the sister of the Great Knight, leader of the Blades.
<--->
I heard Lady Magnessen was accused of spying. Nobody knows where she is now.

Message:  1015
<ce>              Ah, thank you for responding to my letter,
<ce>                     %pcf. I am Lady Brisienna. Let
<ce>              me bring you to date on affairs. The spectre
<ce>                 of King Lysandus haunts the streets of
<ce>               Daggerfall at night. Trying to communicate
<ce>                with him is futile. He will occasionally
<ce>               moan the word "Vengeance," but that is the
<ce>                only coherent word I have ever heard him
<ce>              utter. If you are ever in Daggerfall, do not
<ce>              wander the city at night. You are certain to
<ce>                  be attacked by his legion of ghosts.
<ce>                                    
<ce>                  It would be probably more gainful to
<ce>                investigate those who might have wronged
<ce>             Lysandus to find the cause behind his torment.
<ce>            I do not know if the royal family of Daggerfall
<ce>                or another person or persons merit more
<ce>                suspicion. The major powers of the Bay,
<ce>                Sentinel, Wayrest, and Daggerfall may be
<ce>                         good places to start.
<ce>                             (continued...)

Message:  1016
<ce>              In the matter of the letter, the Emperor's
<ce>             agent says that he was unable to hand-deliver
<ce>             it to the Queen because of the war. He hired a
<ce>              courier who supposedly delivered the letter
<ce>             in his stead. We do not even know the name of
<ce>                this courier. Obviously, there is little
<ce>             information of use, but it would be worthwhile
<ce>              to see whether the letter arrived at Castle
<ce>              Daggerfall at all. How you decide to do this
<ce>             is entirely your decision. I will contact you
<ce>                 if any new information should surface.
<ce>                                    
<ce>             I am leaving Daggerfall soon. My position has
<ce>              here has been compromised and my life is in
<ce>              danger. Do not mention my name in court. It
<ce>              is more likely to hurt than help. Good luck,
<ce>                       and watch your back, %pcf.

Message:  1020
Dear %pcn,

     I heard about your accident at sea, and feared the
 worst. Now that I've heard you're alive and well, I
 would like the opportunity to meet with you and discuss
 our beloved Emperor's mission in the Iliac Bay.

     Allow me to introduce myself. I am Lady Magnessen,
 the Emperor's agent in the court of Daggerfall. My
 position is not so official as an ambassador. None but
 other agents of the Emperor know of my true affiliation.
 The Iliac Bay is rife with rebels against the Imperial
 throne, so your discretion is required.

     For the purpose of our meeting, I will take a room
 at an inn, \_dirtypit\_ in \_\_dirtypit\_
 of Daggerfall, for a month. After that, I will no longer
 be available. I will expect you as soon as possible.

<ce>                            Yours sincerely,

<ce>                       Brisienna, Lady Magnessen

Message:  1021
%pcn,

     I sent you a letter weeks ago. I only hope
 it caught up with you. If this one crosses your path
 after you have visited me on your way to see me in
 \_\_dirtypit\_ please do not take offense.
 As I mentioned in the previous letter, I am the
 Emperor's agent in Daggerfall and it is imperative
 that I speak with you. I have extend my stay at
 \_dirtypit\_ in \_\_dirtypit\_ for two more weeks.

 Of course there is the possibility that you have
 intentionally snubbed me and shirked your duty to
 the Emperor. I hope that is not the case. If you
 fail to arrive, I will be forced to assume you are
 a traitor to his Imperial Majesty, Uriel Septim VII.

<ce>                            Yours sincerely,

<ce>                       Brisienna, Lady Magnessen

Message:  1022
%pcn,

 I waited and you never arrived. I shall report
 this to the Emperor. If I have my way, you will
 be barred from service.

<ce>                             Lady Magnessen

Message:  1026
<ce>             You approached by a young page who smiles in
<ce>             recognition and does not ask your name before
<ce>               he hands you a letter addressed to you. To
<ce>               all of your questions, he merely responds
<ce>                     with a blank look and a smile.
                                     <--->
<ce>             A letter is pressed into your hand. You spin
<ce>             to see who gave it to you. You catch a glimpse
<ce>                of livery that vanishes into the crowd.
                                     <--->
<ce>              A page steps forward saying "Letter for you
<ce>                      %pcn," and hands you a note.
                                     <--->
<ce>              A courier steps up to you with a letter in
<ce>                hand. "I was told to give this letter to
<ce>                someone fitting your description," says
<ce>                the courier. As you take it, the courier
<ce>                     turns and walks quickly away.
                                     <--->
<ce>                Reaching into you pack for something to
<ce>              eat, you spy a note. It wasn't there before.
                                     <--->
<ce>             A redfaced courier startles you with a cry of
<ce>                   "Letter for %pcn! Hey that must be
<ce>                 you. Here, take this. Gotta go. Other
<ce>                         deliveries to be made.

Message:  1025
<ce>             You wake and look around the room. Some hours
<ce>            ago, you were in a boat, en route to Daggerfall,
<ce>              when a storm of supernatural strength boiled
<ce>            over the Iliac Bay like a malefic creature. Your
<ce>              boat was destroyed, but you managed to swim
<ce>            through the churning water to a promontory rock.
<ce>           There you found a cave and escaped the fury of the
<ce>            storm. You had only just lit a small fire when a
<ce>             mudslide sealed you within. Your fear of being
<ce>             buried alive calmed when you saw the corridor
<ce>           leading out of the cavern. Perhaps there is a way
<ce>           out of this cave after all. Once free of the cave,
<ce>                   you can begin the Emperor's quest.


-- Symbols used in the QRC file:
--
--              %pcf occurs 3 times.
--              %pcn occurs 5 times.
--              %qdt occurs 2 times.
--       \_\_dirtypit\_ occurs 11 times.

QBN:
Item \_letter1\_ letter used 1020
Item \_letter2\_ letter used 1021
Item stopMainQuest letter used 1022

Person ladyBrisienna named Lady\_Brisienna anyInfo 1012 rumors 1013

Place PiratesHold permanent PirateerHold1
Place \_dirtypit\_ remote tavern

Clock \_S.00\_ 7.0:0 14.0:0
Clock \_S.01\_ 30.0:0 flag 0:1 range 0 1
Clock \_S.02\_ 14.0:0 flag 0:1 range 0 1
Clock \_oneday\_ 1.0:0 flag 0:1 range 0 1
Clock \_S.07\_ 29.9:0 flag 0:1 range 0 1

--	Quest start-up:
	say 1025
	log 1010 step 0
	pc at PiratesHold do \_S.05\_

\_S.00\_ task:
	place npc ladyBrisienna at \_dirtypit\_
	create npc at \_dirtypit\_
	give pc \_letter1\_ notify 1026
	start timer \_S.01\_

\_S.01\_ task:
	give pc \_letter2\_ notify 1026
	start timer \_S.02\_

\_S.02\_ task:
	give pc stopMainQuest notify 1026
	change repute with ladyBrisienna by -15
	start timer \_S.07\_

\_S.03\_ task:
	clicked npc ladyBrisienna
	say 1015
	say 1016
	log 1011 step 1
	change global state \_S.06\_
	stop timer \_S.00\_
	stop timer \_S.01\_
	stop timer \_S.02\_
	start timer \_oneday\_

\_oneday\_ task:
	end quest

until \_S.05\_ performed:
	start timer \_S.00\_
	start quest 977 977
	start quest 999 999
	change quest status 0

MetLadyBrisienna \_S.06\_
\_S.07\_ task:
	end quest

* * *

### Nocturnal's Quest

The original quest for the Skeleton's Key offered by the daedra Nocturnal has a potentially nasty bug in it.

In the original version, the PC could accept a bribe to betray Nocturnal, who punished the PC by destroying the bribe when she discovered the treachery. Unfortunately the quest scripting was leaky and handling of this betrayal was wonky. The PC was flogged by Qrc messages about bribes he knew nothing about, and so forth. If the PC already possessed the Warlock's Ring artifact, the quest could potentially destroy it.

What follows here is a replacement for Nocturnal's quest that handles the bribery without misleading messages, and as a bonus, sets up the quest so that the PC can accept the bribe and redeem his standing with Nocturnal, thus garnering two artifacts in one quest. (I don't like punative dead-ends unless the game has a functioning mindreader for my characters)

--  Nocturnal's quest for the Skeleton Key artifact.
--
--  Synopsis:
--      Noturnal will bestow the Skeleton Key on the questee for assassinating
--      her mortal enemy \_monster\_ in \_mondung\_.  \_monster\_ in turn will offer
--      either Auriel's Bow or the Warlock's Ring to the questee to betray
--      Nocturnal and spare \_monster\_'s life.  If the questee betrays Nocturnal
--      he'll be plagued by a couple of frost daedra and Nocturnal will destroy
--      whichever alternate artifact the questee pursues.
--
--      If the questee honors Nocturnal's request he must find Nocturnal's
--      aide \_qgfriend\_ to obtain the Skeleton Key after the assassination.
--
--      The original quest from Bethesda offered the Warlock's Ring as a bribe
--      if the questee already had possession of Auriel's Bow, but failed to
--      account for the fact that the questee might indeed have both bribes
--      already.  The present version now handles that better and offers the
--      questee the Staff of Magnus if he already has both the bow and ring.
--
--      The questee may outwit the \_monster\_ and garner both artifacts, the Key
--      and whatever bribe was offered, but only if his gossip repute is high
--      enough.  Unfortunately, the rumor mill apparatus seems not to be
--      working, so for the nonce use letters for breadcrumbs.
--
QuestDir: q/
Quest: 40c00y00
allowdupes: 1
messages: 51
Qrc:

QuestorOffer:  \[1000\]
<ce>                      You wish, %pcn, for power.
<ce>                 With it you can thrash at shadows and
<ce>                   have a hope. If I brought you this
<ce>                  hope, what would you bring me? Would
<ce>                  you destroy one who brings me pain?

RefuseQuest:  \[1001\]
<ce>                As I thought, no heart at all. We shall
<ce>                 have no contract then, you and I, and,
<ce>                   disappointed, return I to my cold
<ce>                 palace. The souls within are no kinder
<ce>                than those here, but there, they follow.

AcceptQuest:  \[1002\]
<ce>                Would you? You would be my anesthesia?
<ce>                You will go to the lair of the \_monster\_
<ce>                 =monster\_ at \_\_\_mondung\_ and slay %g2?
<ce>                    Ah, %pcf, for that I would give
<ce>                      to you my \_artifact\_ which I
<ce>                 have entrusted to my slave \_qgfriend\_.
<ce>                       After =monster\_ lies dead,
<ce>                     go to \_\_\_qgfriend\_ and meet my
<ce>                     slave at \_\_qgfriend\_. You will
<ce>                  recognize %g2 easily - a =qgfriend\_.
<ce>                 There you will receive the \_artifact\_.
<ce>                   And now, I must return to my cold
<ce>                 palace of Oblivion. I will await news.

QuestFail:  \[1003\]
<ce>                      No. You must be looking for
<ce>                        a different =qgfriend\_.
<ce>                       My name isn't \_qgfriend\_.

QuestComplete:  \[1004\]
<ce>                           So, you're %pcn,
<ce>                         right? I'm \_qgfriend\_.
<ce>                I must say, I was wondering what kind of
<ce>                %ra it took to glide into a \_monster\_'s
<ce>                 stronghold, murder the owner, and trip
<ce>                     right on out. Very impressive.
<ce>                   Nocturnal is likewise intrigued by
<ce>                   you, hence this gift: \_artifact\_.
<ce>                       Be careful with it, %pcf.

RumorsDuringQuest:  \[1005\]
=monster\_ is a sneaky one -- %g brings the whole Mages Guild under suspicion.
<--->
I can't trust anyone at the Mages Guild, knowing =monster\_ is a member.
<--->
Mages say =monster\_ is hiding from Oblivion's wrath at \_\_\_fakedung\_.
<--->
Mage's are gathering to scour \_\_\_fakedung\_ for that scoundrel =monster\_.
<--->
I saw a bunch of mages heading out to \_\_\_fakedung\_.
<--->
I heard that traitor =monster\_ is now hiding out in \_\_\_fakedung\_.

RumorsPostfailure:  \[1006\]
I knew =monster\_ was in some kind of trouble, why else would %g leave so quickly?
<--->
The Mages Guild must have decided =monster\_ was a liability to their image.

RumorsPostsuccess:  \[1007\]
Well, I can't say I'm surprised about =monster\_; %g3 past caught up to %g2.
<--->
I don't even think the Archmagister is sad to hear of the death of =monster\_.

QuestorPostsuccess:  \[1008\]
Error: Quest 40c00y00.q tried to display text

QuestorPostsuccess:  \[1008\]
<ce>                                   

QuestorPostfailure:  \[1009\]
Error: Quest 40c00y00.q tried to display text

QuestorPostfailure:  \[1009\]
<ce>                                   

Message:  1011
\_qgfriend\_ is one of the regulars at \_\_qgfriend\_, just %di of here.
<--->
\_qgfriend\_ is a =qgfriend\_ who haunts \_\_qgfriend\_. That's to the %di.

Message:  1012
\_qgfriend\_ is an agent of Nocturnal, operating out of \_\_qgfriend\_ %di of here.
<--->
\_qgfriend\_ is one of the slaves of Nocturnal, Daedra Lady of the Night.

Message:  1013
Nocturnal is the mysterious Lady of the Night, one of the Daedric Regents.
<--->
Nocturnal is a Regent of Oblivion, whose sphere is darkness and mystery.

Message:  1014
The \_artifact\_ can open any lock.

Message: 1015
Mages say =monster\_ is hiding from Oblivion's wrath at \_\_\_fakedung\_.
<--->
Mage's are gathering to scour \_\_\_fakedung\_ for that scoundrel =monster\_.
<--->
I saw a bunch of mages heading out to \_\_\_fakedung\_.
<--->
I heard that traitor =monster\_ is now hiding out in \_\_\_fakedung\_.

Message: 1016
<ce> %oth!  =monster\_,
<ce> Oblivion's agent is upon us!
<ce> I've moved the most valuable reagents to
<ce> to \_\_\_fakedung\_ and
<ce> plan to meet you there after you've
<ce> dealt with that pesky %ra.
<ce>  
<ce> N.

Message: 1017
<ce> A voice seems to echo
<ce> in your ear: =monster\_
<ce> ...sure...special place...
<ce> news...

Message: 1018
%qdt:
 When I arrived at \_\_\_mondung\_
 an echo mentioned a 'special
 place' inside.

Message: 1019
%qdt:
 I found a note from =monster\_ saying that %g is expected
 at \_\_\_fakedung\_ after trying to bribe me.

Message:  1020
%qdt:
 I now have a contract with Nocturnal,
 one of the Daedric Princes of Oblivion. I have agreed
 to slay the \_monster\_ =monster\_ in %g3 stronghold of
 \_\_\_mondung\_ in exchange for Nocturnal's
 artifact, the \_artifact\_. I must have the deed
 done and meet her agent \_qgfriend\_
 in \_\_qgfriend\_ of \_\_\_qgfriend\_
 in =1stparton\_ days or I will have failed the contract.
 This may mean something worse than just me not getting
 the \_artifact\_.

Message:  1021
%qdt:
 =monster\_ offered me \_bow\_ in exchange
 for %g3 life, and I could not resist.
 %g said that it is somewhere in
 \_\_\_bowdung\_.  Since I have already
 betrayed Nocturnal, I may as well
 try to find it.

Message:  1022
%qdt:
 =monster\_ offered me the \_ring\_ in exchange
 for %g3 life, and I could not resist.
 %g said that it is somewhere in
 \_\_\_ringdung\_.  Since I have already
 betrayed Nocturnal, I may as well
 try to find it.

Message:  1023
%qdt:
 =monster\_ offered me the \_staff\_ in exchange
 for %g3 life, and I could not resist.
 %g said that it is somewhere in
 \_staffdung\_.  Since I have already
 betrayed Nocturnal, I may as well
 try to find it.

Message:  1030
<ce>                   Wait!  I know that Nocturnal has
<ce>                  sent you, and that she has promised
<ce>                   you the \_artifact\_.  But if it is
<ce>                  truly power you seek, I can top her
<ce>                   offer -- if you cease your attack,
<ce>                  I will tell you where to find \_bow\_.
<ce>                       Do you agree to my offer?

Message:  1031
<ce>                   Wait!  I know that Nocturnal has
<ce>                  sent you, and that she has promised
<ce>                   you the \_artifact\_.  But if it is
<ce>                  truly power you seek, I can top her
<ce>                   offer -- if you cease your attack,
<ce>               I will tell you where to find the \_ring\_.
<ce>                       Do you agree to my offer?

Message:  1032
<ce>                   Wait!  I know that Nocturnal has
<ce>                  sent you, and that she has promised
<ce>                   you the \_artifact\_.  But if it is
<ce>                  truly power you seek, I can top her
<ce>                   offer -- if you cease your attack,
<ce>               I will tell you where to find the \_staff\_.
<ce>                       Do you agree to my offer?

Message:  1035
<ce>                       You have slain =monster\_.
<ce>                       Prince Nocturnal will now
<ce>                              rest easier.

Message:  1040
<ce>                      Excellent.  I knew you had
<ce>                      a mercenary's heart.  \_bow\_
<ce>                      lies hidden in \_\_\_bowdung\_.
<ce>                      You have only to find it and
<ce>                       you will be a power to be
<ce>                      reckoned with indeed.  Now,
<ce>                      I bid you good day, lest you
<ce>                       decide to claim both \_bow\_
<ce>                         and the \_artifact\_ by
<ce>                     killing me.  A mercenary heart
<ce>                      such as yours should not be
<ce>                            trusted too far!

Message:  1041
<ce>                      Excellent.  I knew you had
<ce>                    a mercenary's heart.  The \_ring\_
<ce>                      lies hidden in \_\_\_ringdung\_.
<ce>                      You have only to find it and
<ce>                       you will be a power to be
<ce>                      reckoned with indeed.  Now,
<ce>                      I bid you good day, lest you
<ce>                    decide to claim both the \_ring\_
<ce>                         and the \_artifact\_ by
<ce>                     killing me.  A mercenary heart
<ce>                      such as yours should not be
<ce>                            trusted too far!

Message:  1042
<ce>                      Excellent.  I knew you had
<ce>                    a mercenary's heart.  The \_staff\_
<ce>                      lies hidden in \_staffdung\_.
<ce>                      You have only to find it and
<ce>                       you will be a power to be
<ce>                      reckoned with indeed.  Now,
<ce>                      I bid you good day, lest you
<ce>                    decide to claim both the \_staff\_
<ce>                         and the \_artifact\_ by
<ce>                     killing me.  A mercenary heart
<ce>                      such as yours should not be
<ce>                            trusted too far!

QuestTimeLapse:  \[1045\]
<ce>                 Mocking laughter echoes in your mind,
<ce>                                    
<ce>                   "You should have known better than
<ce>                    to trust me, %pcn.  Perhaps this
<ce>                    shadow of power can satisfy your
<ce>                     craving for a time, while you
<ce>                     rage vainly against those who
<ce>                 actually possess it.  Me for example.
<ce>                   I do hope sweet Nocturnal won't be
<ce>                  too displeased with your treachery."
<ce>                                    
<ce>                  \_bow\_ crumbles to dust in your hand.

Message:  1046
<ce>                 Mocking laughter echoes in your mind,
<ce>                                    
<ce>                   "You should have known better than
<ce>                    to trust me, %pcn.  Perhaps this
<ce>                    shadow of power can satisfy your
<ce>                     craving for a time, while you
<ce>                     rage vainly against those who
<ce>                 actually possess it.  Me for example.
<ce>                   I do hope sweet Nocturnal won't be
<ce>                  too displeased with your treachery."
<ce>                                    
<ce>               The \_ring\_ crumbles to dust in your hand.

Message:  1047
<ce>                 Mocking laughter echoes in your mind,
<ce>                                    
<ce>                   "You should have known better than
<ce>                    to trust me, %pcn.  Perhaps this
<ce>                    shadow of power can satisfy your
<ce>                     craving for a time, while you
<ce>                     rage vainly against those who
<ce>                 actually possess it.  Me for example.
<ce>                   I do hope sweet Nocturnal won't be
<ce>                  too displeased with your treachery."
<ce>                                    
<ce>               The \_staff\_ crumbles to dust in your hand.

Message:  1050
<ce>                 Nocturnal was most impressed at your
<ce>                     daring, %pcn.  She asked me to
<ce>                    kill you quickly as a reward for
<ce>                       such audacious treachery.

Qbn:

Item \_artifact\_ artifact Skeletons\_Key used 1014
Item \_bow\_      artifact Auriels\_Bow
Item \_ring\_     artifact Warlocks\_Ring
Item \_staff\_    artifact Staff\_of\_Magnus
Item \_letter\_ letter used 1016

Questor \_questgiver\_ pic 0x70 anyInfo 1013
Person \_qgfriend\_ group Knightly\_Order anyInfo 1011 rumors 1012

Place \_mondung\_ remote dungeon
Place \_bowdung\_ remote dungeon
Place \_ringdung\_ remote dungeon
Place \_staffdung\_ remote dungeon
Place \_fakedung\_ remote dungeon

Clock \_1stparton\_
Clock delay 1:0
Clock plague 1.0:0 2.18:40 flag 1
Clock betrayQuestor 0:2 0:30

Foe \_monster\_ is mage
Foe \_daedra\_ is Frost\_daedra

--  Quest startup:
    start timer \_1stparton\_
    log 1020 step 0
    reveal \_mondung\_
    place \_monster\_ at \_mondung\_
    have \_bow\_ set priorBow
    have \_ring\_ set priorRing
    \_letter\_ used do readLetter

--  Give him a hint on his first visit to the \_monster\_'s dungeon.
--  Once he's been bribed or killed it, be silent.

atDung when pc at \_mondung\_

giveHint task:
    when atDung and not mondead and not bribe
    say 1017
    log 1018 step 1

--  Once the PC reads the clue about the alternate hide-out, mark it on
--  the overhead map.

readLetter task:
    reveal \_fakedung\_
    log 1019 step 1

\_1stparton\_ task:
    end quest

npcclicked task:
    clicked \_qgfriend\_

mondead task:
    killed \_monster\_
    say 1035

pcgetsgold task:
    when npcclicked and mondead

--  The difference between 'unset' and 'clear' is that 'clear' would allow
--  repeated clicks of the npc (and thus repeated artifact gifts).  Unset
--  acts as a perpetual clear so that future clicks are ignored, effectly
--  dropping the npcclicked task on the floor.

    unset npcclicked
    give pc \_artifact\_
    start timer delay

injury task:
    injured \_monster\_
    place \_letter\_ at \_mondung\_

variable priorBow
variable priorRing
betrayQuestor task

--  Case selection for the various artifact bribes once the PC acquires one.
--  Take care to quash the marker that used to check for prior possession
--  of the artifact, since acquiring the artifact will perforce make it
--  true now, which is not what it is suppose to indicate.  (Note there is
--  no path to this task if the PC possesses the artifact prior to starting
--  this quest, at least assuming this is the only quest that bestows the
--  the Staff of Magnus, which is probably false, but what the original
--  quest author assumed)

findBow task:
    clicked \_bow\_
    clear priorBow

findRing task:
    clicked \_ring\_
    clear priorRing

findStaff task:
    clicked \_staff\_

--  Case selection for accepting one of the artifact bribes from \_monster\_.

betrayBow task:
    when bribe and bribeBow
    say 1040
    log 1021 step 2
    reveal \_bowdung\_
    place \_bow\_ at \_bowdung\_

betrayRing task:
    when bribe and bribeRing
    say 1041
    log 1022 step 2
    reveal \_ringdung\_
    place \_ring\_ at \_ringdung\_

betrayStaff task:
    when bribe and bribeStaff
    say 1042
    log 1023 step 2
    reveal \_staffdung\_
    place \_staff\_ at \_staffdung\_

--  Once the PC injures the \_monster\_, offer the PC a bribe of another
--  artifact to desist.  In its haste to get away from the PC, \_monster\_
--  drops a scrap of paper that the PC may read.

bribe task:
    repute \_questgiver\_ is -20
    start timer plague
    place \_monster\_ at \_fakedung\_
    place \_letter\_ at \_mondung\_
--  This command doesn't appear to have any effect here.
    rumor mill 1015
    add dialog for location \_fakedung\_

noBribe task:

--  Case selection to offer an alternative artifact as a bribe to make the
--  PC desist.  If the PC doesn't already have Auriel's Bow, offer it.
--  Else offer the Warlock's Ring if the PC already has the bow.  And
--  offer the Staff of Magnus if the PC already has both the bow and ring.
--  Unless the user has installed the CompUSA quest pack, we're guaranteed
--  that the PC doesn't already have the Staff.

bribeBow task:
    when injury and not priorBow
    prompt 1030 yes bribe no noBribe

bribeRing task:
    when injury and priorBow and not priorRing
    prompt 1031 yes bribe no noBribe

bribeStaff task:
    when injury and priorRing and priorBow
    prompt 1032 yes bribe no noBribe

--  Nocturnal understandably gets a bit anxious if the PC accepts the
--  bribe, even if he ultimately kills \_monster\_.  The frost daedra
--  won't pose a big threat except to very junior PCs, and even so,
--  the generator often fails to spawn any of them  (I wonder why.  I've
--  encountered \_daedra\_ only once during testing.  And then only because
--  I deliberately slept in an inn as soon as the plague task had
--  triggered)

plague task:
    create foe \_daedra\_ every 3 minutes 4 times with 100% success

boast task:
    injured \_daedra\_
    say 1050

--  But, if the PC takes possession of the bribe before dispatching \_monster\_
--  Nocturnal will acknowledge the deception by destroying the artifact bribe.
--  If the PC does dispatch \_monster\_ before the timers run out, he can
--  recover the destroyed artifact from the \_fakedung\_.

byeBow task:
    when betrayQuestor and not priorBow
    place \_bow\_ at \_fakedung\_
    say 1045

byeRing task:
    when betrayQuestor and priorBow and not priorRing
    place \_ring\_ at \_fakedung\_
    say 1046

byeStaff task:
    when betrayQuestor and priorBow and priorRing
    place \_staff\_ at \_fakedung\_
    say 1047

--  Case selection to detect (for Nocturnal) that the PC has apparently
--  honored the \_nonster\_'s bribe.

betrayedForBow task:
    when findBow and not mondead
    start timer betrayQuestor

betrayedForRing task:
    when findRing and not mondead
    start timer betrayQuestor

betrayedForStaff task:
    when findStaff and not mondead
    start timer betrayQuestor

--  On the other hand, if the PC has first dispatched \_monster\_ for Nocturnal,
--  allow the PC to garner the artifact bribe before collecting the Key
--  from Nocturnal's slavey.

saveBow task:
    when findBow and mondead
    make \_bow\_ permanent

saveRing task:
    when findRing and mondead
    make \_ring\_ permanent

saveStaff task:
    when findStaff and mondead
    make \_staff\_ permanent

--  Once the Key is collected, allow the PC to bask in the post-quest
--  rumors of success for a bit before finally ending the quest.

delay task:
    end quest

* * *

Acknowledgments
---------------

The present work builds on the pioneering efforts from many individuals, including, but not limited to, the following

    Danibene - [\[email protected\]](/cdn-cgi/l/email-protection)
    Dave Humphrey - [\[email protected\]](/cdn-cgi/l/email-protection)
    Glen Wright - [\[email protected\]](/cdn-cgi/l/email-protection)
    J. Carl Smith - [\[email protected\]](/cdn-cgi/l/email-protection)
    James Dean - ([\[email protected\]](/cdn-cgi/l/email-protection))
    Kevin Schoemehl - [\[email protected\]](/cdn-cgi/l/email-protection)
    Leonard Miyata - [\[email protected\]](/cdn-cgi/l/email-protection)
    Lord Phoenix - [\[email protected\]](/cdn-cgi/l/email-protection)
    m00se - [\[email protected\]](/cdn-cgi/l/email-protection)
    Mark Haman - ([\[email protected\]](/cdn-cgi/l/email-protection))
    Mark Holden - ([\[email protected\]](/cdn-cgi/l/email-protection)
    Mark Kennedy - [\[email protected\]](/cdn-cgi/l/email-protection)
    Matthew O. Avent - [\[email protected\]](/cdn-cgi/l/email-protection)
    Michael Schneider - [\[email protected\]](/cdn-cgi/l/email-protection)
    [\[email protected\]](/cdn-cgi/l/email-protection)
    Peggy Hanks - [\[email protected\]](/cdn-cgi/l/email-protection)
    Rick Huebner - ([\[email protected\]](/cdn-cgi/l/email-protection))
    Shadow Jack - [\[email protected\]](/cdn-cgi/l/email-protection)
    Silent - [\[email protected\]](/cdn-cgi/l/email-protection)
    Skeleton Man
    Thomas Hautesserres - ([\[email protected\]](/cdn-cgi/l/email-protection))
    Thor-Eirik Larsen - [\[email protected\]](/cdn-cgi/l/email-protection)

Thanks also to

    Magnus Itland - [\[email protected\]](/cdn-cgi/l/email-protection)

for relentless play-testing and encouragement.

And especially  

**George Spenser Brown**


for his proof that the theory of types is unnecessary, without which the present work could not have been accomplished.

* * *

Donald Tipton  
[\[email protected\]](/cdn-cgi/l/email-protection)  
November, 2000
