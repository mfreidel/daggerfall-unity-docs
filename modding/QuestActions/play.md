# play

## Playing sounds

```
play sound aSound every time1 minutes time2 times
play sound aSound time1 time2
```

This action plays one of the 459 **Daggerfall** [sound effects](../../api/DaggerfallWorkshop.SoundClips.yml) once every `time1` game minutes while the task containing the `play sound` command is active. Specifying `time2` doesn't seem to have any effect on the behavior of the command, though.

Only the first form will accept one of the catalogued names for a sound effect, although the pure number for the effect may be used with either form.

Every model creature in Daggerfall has three separate sound effects associated with it: _distant_, _nearby_, and _in combat_. Other sound effects include various environmental conditions, like barking dogs, slamming doors, and, of course, _Halt! Halt!_

There are symbolic names for all the model creature sounds, but I haven't yet completed the environmental catalog, whose interpretation is much more subjective as to what to call the collection of assorted grunts, wheezes, and eerie noises.

Also, all the sound effects from **TES1, Arena** appear to be present. So if you've missed **Fire Daemons** or **Iron Golems**, you can at least use their respective noises.


## Playing video clips

```
play video nn
```

This action plays a prerecorded video clip associated with the main story quest, where `nn` indicates which of the `anim00_nn_.vid` clips in the **arena2** directory to use.

The standard quests from Bethesda mention videos 3, 5-10, 13-15, so 0-2, 4, 11, 12 are either played automatically at certain milestones, like player death or 'bite me', or have never before been seen.
