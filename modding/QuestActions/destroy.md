# destroy

> Change an NPC's status

```
destroy anNPC
```

This action completely obliterates a quest _Person_ resource. This is different from [_hide npc_](./hide.md) in that with the latter form, the resource symbol is still available for substitution within Qrc messages blocks.

Once a quest resource has been destroyed, however, any subsequent reference to that quest resource symbol by a Qrc message will yield the infamous **BLANK** text.

This action appears in the mummy's finger quest, one of the vampire clan quests, and in Mynisera's quest to locate the courier who delivered the Emperor's letter to Aubk-i.
