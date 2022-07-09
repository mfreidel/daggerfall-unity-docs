# Reusing Quest elements

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
