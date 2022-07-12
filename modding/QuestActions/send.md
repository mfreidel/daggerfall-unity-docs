# send

> Creating waves of attacking foes

    send aFoe every m minutes n times with k% success

The _send foe_ command operates like the `[_create foe_](#createfoe)` command, but limits the spawning of _aFoe_ to the times when the player is above ground, generally near towns.

    Foe \_zombie\_ is Zombie

    \_doneCyndassa\_ task:
        quest 7 completed

    \_deliverLetter\_ task:
    	when \_doneWith7\_
        send \_zombie\_ every 1410 minutes -1 times with 100% success

is an excerpt from the main story where the **King of Worms** delivers his invitation to revisit **Scourg Burrow** at the start of the _Soul of the Lich_ quest.
