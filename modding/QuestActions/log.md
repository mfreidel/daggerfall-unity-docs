# log

> Entering messages in the log book

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

See the `[remove log step](#changeglobal2)` action to obliterate a log book entry.
