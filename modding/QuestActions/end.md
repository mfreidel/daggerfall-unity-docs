# end

> Ending the current quest

```
end quest
```

Any task may specify that it completes the quest's activity, whether successful or not, by using the end quest command. When the _end quest_ is performed, all quest-related objects (green background) in the player's inventory are removed, other quest resources (Items, Persons, Places, and Foes) are discarded, and the player's log book emptied.

Every well-written side quest usually has an end to it. Many quests employ the common idiom of a set time to complete the quest in, and self-terminate if the job isn't completed in time.


### Example

The following example specifies a time limit of 7 to 11 days for the quest. If the PC hasn't made good on the quest goal in that time, the quest ends when that clock runs down.

```
Clock _timeLimit_ 7.0:0 11.0:0

_timeLimit_ task:
    end quest
```
