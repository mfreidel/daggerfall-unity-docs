# Common pitfalls

There are a number of pitfalls to using the X-engine for quests. This section documents known issues with the quest engine, and tries to explain them in a general way. Broadly stated, X-engine cannot reliably handle simultaneous events, and imposes a serial or chronological order that the quest author does not realize.

Technically, these are sometimes called _race conditions_, because, like the winner of a foot race, the task which executes first cannot be predicted by the quest author.

### Unable to check repute

Using the `[when repute with _aFaction_](#factionrepute)` condition to examine the PC's standing with a Daggerfall faction can fail spontaneously when the result of that task is examined in a subsequent `[when condition](#multipletests)`.

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

Unlike message posts, a sound once started by the `[play sound](#playsound)` command performs indefinitely.

To stop a playing sound, clear the task that started it.

    Clock \_silence\_

    \_makeNoise\_ task:
        play sound halt every 2 minutes 3 times

    \_silence\_ task:
        clear \_makeNoise\_

In this quest fragment, when the \_silence\_ timer runs down, the noise maker task is stopped.
