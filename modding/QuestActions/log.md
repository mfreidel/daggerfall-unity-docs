# log

> Entering messages in the log book

```
log nnnn step i
```

> [!IMPORTANT]
> The number of active steps in a single quest is limited to 10, so `i` must be between 0 and 9.

This action adds a Qrc message nnnn to the player's log book as the i<sup>th</sup> entry for this quest. Quest entries start from number 0. A log action which reuses an entry number `i` replaces the previous log entry for that step with the current one. When the quest ends, the log entries added by the quest are removed from the log book.

Since the PC may undertake several quests simultaneously, the **X-engine** uses the entry number to collect the messages of one quest in the same area of the log book.

> [!NOTE]
> The number of concurrently active quests is limited to 32.


## Example

```
Message: 1020
%qdt:
  I have accepted a quest from _qgiver_.

Message: 1021
%qdt:
  A friend of _qgiver_ has directed me to __house_.

Message: 1022
%qdt:
  I've discovered  _qgiver_'s _item_ might be hidden
  in ___dungeon_.

--  Quest startup:
    log 1020 step 0

when clicked _friend_
    log 1021 step 1

when used _letter_
    log 1022 step 1
```

When this quest begins, message 1020 is added to the player's log book as the first message of this quest. Once the PC locates the quest giver's friend, a second message, 1021 is added to the log book. Later, once the PC reads a quest-related letter, this second message is replaced with message 1022.

See the [remove log step](./remove.md) action to obliterate a log book entry.
