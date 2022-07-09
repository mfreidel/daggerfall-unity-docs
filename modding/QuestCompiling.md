# Quest compiling and decompiling

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

In addition, the resulting quest scenario will contain the information required to use the **X-engine** quest debugger. See **Using the X-Engine quest debugger** section for more information.

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
