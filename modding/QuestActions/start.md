# start

## Starting other quests

    start quest nnnn mmmm

This action starts up one or more quests from the S000_nnnn_ family of quests. In the existing quests from Bethesda, _nnnn_ and _mmmm_ are always the same number.


## Starting a task in another quest

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


## Starting a clock

    start timer aClock

This action (re)starts the specified timer.
