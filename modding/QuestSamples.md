Sample quests
-------------

### Lady Brisienna

Lady Brisienna starts the player character off on the first quest when the game begins. The decompiler renders her quest as follows.

-- Quest: /dos/g/game/dag/arena2/\_brisien.qbn.
-- StartsBy: letter
-- Questee: anyone
-- Repute: 0
-- QuestId: 0
Quest: \_brisien
-- Message panels
QRC:

RefuseQuest:  \[1001\]
Error: Questtext 1001 called on BRISIEN.Q

AcceptQuest:  \[1002\]
Error: Questtext 1002 called on BRISIEN.Q

QuestFail:  \[1003\]
Error: Questtext 1003 called on BRISIEN.Q

RumorsDuringQuest:  \[1005\]
The prophets say that soon what was unclaimed will be claimed.
<--->
Lady Brisienna awaits someone at \_dirtypit\_ in \_\_dirtypit\_.
<--->
That was some storm. Magically created, many are saying.
<--->
Lady Brisienna has taken up lodging at \_dirtypit\_ in \_\_dirtypit\_.

RumorsPostfailure:  \[1006\]
Lady Brisienna has left \_dirtypit\_, muttering about traitors to the Empire.

RumorsPostsuccess:  \[1007\]
Lady Brisienna has left \_dirtypit\_. She must've found who she was looking for.

QuestorPostsuccess:  \[1008\]
<ce>        I hope your investigations are going well so far, %pcf.
<ce>                                    
<ce>                               -- Lady B.

QuestLogEntry:  \[1010\]
%qdt:
 I am on a mission from the emperor to investigate
 the shade of King Lysandus. His spirit has been
 haunting the city of Daggerfall. The emperor
 himself has charged me with the duty of laying
 his ghost to rest.
    There is also the minor matter of a letter
 he sent to the Queen of Daggerfall. If I should
 find out what happened to the letter, he would be
 most appreciative.
    Before landing in Daggerfall, a sudden storm
 capsized the ship. I barely made it into this cave.

Message:  1011
%qdt:
 I met with Lady Brisienna in a tavern room. She
 told me that the three major powers of the Iliac
 bay are Daggerfall, Wayrest, and Sentinel. If I
 am to investigate the mystery of Lysandus' ghost,
 I should start by speaking with the royal families
 of these fiefdoms.

Message:  1012
Lady Magnessen? She's the emmisary of the Blades in Daggerfall.
<--->
Lady Magnessen is a part of the set regularly at Castle Daggerfall.
<--->
Lady Magnessen was formerly in the employ of Mynisera, when she was Queen.

Message:  1013
Lady Magnessen once worked for the Queen Mother, but always for the Emperor.
<--->
Lady Magnessen is the sister of the Great Knight, leader of the Blades.
<--->
I heard Lady Magnessen was accused of spying. Nobody knows where she is now.

Message:  1015
<ce>              Ah, thank you for responding to my letter,
<ce>                     %pcf. I am Lady Brisienna. Let
<ce>              me bring you to date on affairs. The spectre
<ce>                 of King Lysandus haunts the streets of
<ce>               Daggerfall at night. Trying to communicate
<ce>                with him is futile. He will occasionally
<ce>               moan the word "Vengeance," but that is the
<ce>                only coherent word I have ever heard him
<ce>              utter. If you are ever in Daggerfall, do not
<ce>              wander the city at night. You are certain to
<ce>                  be attacked by his legion of ghosts.
<ce>                                    
<ce>                  It would be probably more gainful to
<ce>                investigate those who might have wronged
<ce>             Lysandus to find the cause behind his torment.
<ce>            I do not know if the royal family of Daggerfall
<ce>                or another person or persons merit more
<ce>                suspicion. The major powers of the Bay,
<ce>                Sentinel, Wayrest, and Daggerfall may be
<ce>                         good places to start.
<ce>                             (continued...)

Message:  1016
<ce>              In the matter of the letter, the Emperor's
<ce>             agent says that he was unable to hand-deliver
<ce>             it to the Queen because of the war. He hired a
<ce>              courier who supposedly delivered the letter
<ce>             in his stead. We do not even know the name of
<ce>                this courier. Obviously, there is little
<ce>             information of use, but it would be worthwhile
<ce>              to see whether the letter arrived at Castle
<ce>              Daggerfall at all. How you decide to do this
<ce>             is entirely your decision. I will contact you
<ce>                 if any new information should surface.
<ce>                                    
<ce>             I am leaving Daggerfall soon. My position has
<ce>              here has been compromised and my life is in
<ce>              danger. Do not mention my name in court. It
<ce>              is more likely to hurt than help. Good luck,
<ce>                       and watch your back, %pcf.

Message:  1020
Dear %pcn,

     I heard about your accident at sea, and feared the
 worst. Now that I've heard you're alive and well, I
 would like the opportunity to meet with you and discuss
 our beloved Emperor's mission in the Iliac Bay.

     Allow me to introduce myself. I am Lady Magnessen,
 the Emperor's agent in the court of Daggerfall. My
 position is not so official as an ambassador. None but
 other agents of the Emperor know of my true affiliation.
 The Iliac Bay is rife with rebels against the Imperial
 throne, so your discretion is required.

     For the purpose of our meeting, I will take a room
 at an inn, \_dirtypit\_ in \_\_dirtypit\_
 of Daggerfall, for a month. After that, I will no longer
 be available. I will expect you as soon as possible.

<ce>                            Yours sincerely,

<ce>                       Brisienna, Lady Magnessen

Message:  1021
%pcn,

     I sent you a letter weeks ago. I only hope
 it caught up with you. If this one crosses your path
 after you have visited me on your way to see me in
 \_\_dirtypit\_ please do not take offense.
 As I mentioned in the previous letter, I am the
 Emperor's agent in Daggerfall and it is imperative
 that I speak with you. I have extend my stay at
 \_dirtypit\_ in \_\_dirtypit\_ for two more weeks.

 Of course there is the possibility that you have
 intentionally snubbed me and shirked your duty to
 the Emperor. I hope that is not the case. If you
 fail to arrive, I will be forced to assume you are
 a traitor to his Imperial Majesty, Uriel Septim VII.

<ce>                            Yours sincerely,

<ce>                       Brisienna, Lady Magnessen

Message:  1022
%pcn,

 I waited and you never arrived. I shall report
 this to the Emperor. If I have my way, you will
 be barred from service.

<ce>                             Lady Magnessen

Message:  1026
<ce>             You approached by a young page who smiles in
<ce>             recognition and does not ask your name before
<ce>               he hands you a letter addressed to you. To
<ce>               all of your questions, he merely responds
<ce>                     with a blank look and a smile.
                                     <--->
<ce>             A letter is pressed into your hand. You spin
<ce>             to see who gave it to you. You catch a glimpse
<ce>                of livery that vanishes into the crowd.
                                     <--->
<ce>              A page steps forward saying "Letter for you
<ce>                      %pcn," and hands you a note.
                                     <--->
<ce>              A courier steps up to you with a letter in
<ce>                hand. "I was told to give this letter to
<ce>                someone fitting your description," says
<ce>                the courier. As you take it, the courier
<ce>                     turns and walks quickly away.
                                     <--->
<ce>                Reaching into you pack for something to
<ce>              eat, you spy a note. It wasn't there before.
                                     <--->
<ce>             A redfaced courier startles you with a cry of
<ce>                   "Letter for %pcn! Hey that must be
<ce>                 you. Here, take this. Gotta go. Other
<ce>                         deliveries to be made.

Message:  1025
<ce>             You wake and look around the room. Some hours
<ce>            ago, you were in a boat, en route to Daggerfall,
<ce>              when a storm of supernatural strength boiled
<ce>            over the Iliac Bay like a malefic creature. Your
<ce>              boat was destroyed, but you managed to swim
<ce>            through the churning water to a promontory rock.
<ce>           There you found a cave and escaped the fury of the
<ce>            storm. You had only just lit a small fire when a
<ce>             mudslide sealed you within. Your fear of being
<ce>             buried alive calmed when you saw the corridor
<ce>           leading out of the cavern. Perhaps there is a way
<ce>           out of this cave after all. Once free of the cave,
<ce>                   you can begin the Emperor's quest.


-- Symbols used in the QRC file:
--
--              %pcf occurs 3 times.
--              %pcn occurs 5 times.
--              %qdt occurs 2 times.
--       \_\_dirtypit\_ occurs 11 times.

QBN:
Item \_letter1\_ letter used 1020
Item \_letter2\_ letter used 1021
Item stopMainQuest letter used 1022

Person ladyBrisienna named Lady\_Brisienna anyInfo 1012 rumors 1013

Place PiratesHold permanent PirateerHold1
Place \_dirtypit\_ remote tavern

Clock \_S.00\_ 7.0:0 14.0:0
Clock \_S.01\_ 30.0:0 flag 0:1 range 0 1
Clock \_S.02\_ 14.0:0 flag 0:1 range 0 1
Clock \_oneday\_ 1.0:0 flag 0:1 range 0 1
Clock \_S.07\_ 29.9:0 flag 0:1 range 0 1

--	Quest start-up:
	say 1025
	log 1010 step 0
	pc at PiratesHold do \_S.05\_

\_S.00\_ task:
	place npc ladyBrisienna at \_dirtypit\_
	create npc at \_dirtypit\_
	give pc \_letter1\_ notify 1026
	start timer \_S.01\_

\_S.01\_ task:
	give pc \_letter2\_ notify 1026
	start timer \_S.02\_

\_S.02\_ task:
	give pc stopMainQuest notify 1026
	change repute with ladyBrisienna by -15
	start timer \_S.07\_

\_S.03\_ task:
	clicked npc ladyBrisienna
	say 1015
	say 1016
	log 1011 step 1
	change global state \_S.06\_
	stop timer \_S.00\_
	stop timer \_S.01\_
	stop timer \_S.02\_
	start timer \_oneday\_

\_oneday\_ task:
	end quest

until \_S.05\_ performed:
	start timer \_S.00\_
	start quest 977 977
	start quest 999 999
	change quest status 0

MetLadyBrisienna \_S.06\_
\_S.07\_ task:
	end quest

* * *

### Nocturnal's Quest

The original quest for the Skeleton's Key offered by the daedra Nocturnal has a potentially nasty bug in it.

In the original version, the PC could accept a bribe to betray Nocturnal, who punished the PC by destroying the bribe when she discovered the treachery. Unfortunately the quest scripting was leaky and handling of this betrayal was wonky. The PC was flogged by Qrc messages about bribes he knew nothing about, and so forth. If the PC already possessed the Warlock's Ring artifact, the quest could potentially destroy it.

What follows here is a replacement for Nocturnal's quest that handles the bribery without misleading messages, and as a bonus, sets up the quest so that the PC can accept the bribe and redeem his standing with Nocturnal, thus garnering two artifacts in one quest. (I don't like punative dead-ends unless the game has a functioning mindreader for my characters)

--  Nocturnal's quest for the Skeleton Key artifact.
--
--  Synopsis:
--      Noturnal will bestow the Skeleton Key on the questee for assassinating
--      her mortal enemy \_monster\_ in \_mondung\_.  \_monster\_ in turn will offer
--      either Auriel's Bow or the Warlock's Ring to the questee to betray
--      Nocturnal and spare \_monster\_'s life.  If the questee betrays Nocturnal
--      he'll be plagued by a couple of frost daedra and Nocturnal will destroy
--      whichever alternate artifact the questee pursues.
--
--      If the questee honors Nocturnal's request he must find Nocturnal's
--      aide \_qgfriend\_ to obtain the Skeleton Key after the assassination.
--
--      The original quest from Bethesda offered the Warlock's Ring as a bribe
--      if the questee already had possession of Auriel's Bow, but failed to
--      account for the fact that the questee might indeed have both bribes
--      already.  The present version now handles that better and offers the
--      questee the Staff of Magnus if he already has both the bow and ring.
--
--      The questee may outwit the \_monster\_ and garner both artifacts, the Key
--      and whatever bribe was offered, but only if his gossip repute is high
--      enough.  Unfortunately, the rumor mill apparatus seems not to be
--      working, so for the nonce use letters for breadcrumbs.
--
QuestDir: q/
Quest: 40c00y00
allowdupes: 1
messages: 51
Qrc:

QuestorOffer:  \[1000\]
<ce>                      You wish, %pcn, for power.
<ce>                 With it you can thrash at shadows and
<ce>                   have a hope. If I brought you this
<ce>                  hope, what would you bring me? Would
<ce>                  you destroy one who brings me pain?

RefuseQuest:  \[1001\]
<ce>                As I thought, no heart at all. We shall
<ce>                 have no contract then, you and I, and,
<ce>                   disappointed, return I to my cold
<ce>                 palace. The souls within are no kinder
<ce>                than those here, but there, they follow.

AcceptQuest:  \[1002\]
<ce>                Would you? You would be my anesthesia?
<ce>                You will go to the lair of the \_monster\_
<ce>                 =monster\_ at \_\_\_mondung\_ and slay %g2?
<ce>                    Ah, %pcf, for that I would give
<ce>                      to you my \_artifact\_ which I
<ce>                 have entrusted to my slave \_qgfriend\_.
<ce>                       After =monster\_ lies dead,
<ce>                     go to \_\_\_qgfriend\_ and meet my
<ce>                     slave at \_\_qgfriend\_. You will
<ce>                  recognize %g2 easily - a =qgfriend\_.
<ce>                 There you will receive the \_artifact\_.
<ce>                   And now, I must return to my cold
<ce>                 palace of Oblivion. I will await news.

QuestFail:  \[1003\]
<ce>                      No. You must be looking for
<ce>                        a different =qgfriend\_.
<ce>                       My name isn't \_qgfriend\_.

QuestComplete:  \[1004\]
<ce>                           So, you're %pcn,
<ce>                         right? I'm \_qgfriend\_.
<ce>                I must say, I was wondering what kind of
<ce>                %ra it took to glide into a \_monster\_'s
<ce>                 stronghold, murder the owner, and trip
<ce>                     right on out. Very impressive.
<ce>                   Nocturnal is likewise intrigued by
<ce>                   you, hence this gift: \_artifact\_.
<ce>                       Be careful with it, %pcf.

RumorsDuringQuest:  \[1005\]
=monster\_ is a sneaky one -- %g brings the whole Mages Guild under suspicion.
<--->
I can't trust anyone at the Mages Guild, knowing =monster\_ is a member.
<--->
Mages say =monster\_ is hiding from Oblivion's wrath at \_\_\_fakedung\_.
<--->
Mage's are gathering to scour \_\_\_fakedung\_ for that scoundrel =monster\_.
<--->
I saw a bunch of mages heading out to \_\_\_fakedung\_.
<--->
I heard that traitor =monster\_ is now hiding out in \_\_\_fakedung\_.

RumorsPostfailure:  \[1006\]
I knew =monster\_ was in some kind of trouble, why else would %g leave so quickly?
<--->
The Mages Guild must have decided =monster\_ was a liability to their image.

RumorsPostsuccess:  \[1007\]
Well, I can't say I'm surprised about =monster\_; %g3 past caught up to %g2.
<--->
I don't even think the Archmagister is sad to hear of the death of =monster\_.

QuestorPostsuccess:  \[1008\]
Error: Quest 40c00y00.q tried to display text

QuestorPostsuccess:  \[1008\]
<ce>                                   

QuestorPostfailure:  \[1009\]
Error: Quest 40c00y00.q tried to display text

QuestorPostfailure:  \[1009\]
<ce>                                   

Message:  1011
\_qgfriend\_ is one of the regulars at \_\_qgfriend\_, just %di of here.
<--->
\_qgfriend\_ is a =qgfriend\_ who haunts \_\_qgfriend\_. That's to the %di.

Message:  1012
\_qgfriend\_ is an agent of Nocturnal, operating out of \_\_qgfriend\_ %di of here.
<--->
\_qgfriend\_ is one of the slaves of Nocturnal, Daedra Lady of the Night.

Message:  1013
Nocturnal is the mysterious Lady of the Night, one of the Daedric Regents.
<--->
Nocturnal is a Regent of Oblivion, whose sphere is darkness and mystery.

Message:  1014
The \_artifact\_ can open any lock.

Message: 1015
Mages say =monster\_ is hiding from Oblivion's wrath at \_\_\_fakedung\_.
<--->
Mage's are gathering to scour \_\_\_fakedung\_ for that scoundrel =monster\_.
<--->
I saw a bunch of mages heading out to \_\_\_fakedung\_.
<--->
I heard that traitor =monster\_ is now hiding out in \_\_\_fakedung\_.

Message: 1016
<ce> %oth!  =monster\_,
<ce> Oblivion's agent is upon us!
<ce> I've moved the most valuable reagents to
<ce> to \_\_\_fakedung\_ and
<ce> plan to meet you there after you've
<ce> dealt with that pesky %ra.
<ce>  
<ce> N.

Message: 1017
<ce> A voice seems to echo
<ce> in your ear: =monster\_
<ce> ...sure...special place...
<ce> news...

Message: 1018
%qdt:
 When I arrived at \_\_\_mondung\_
 an echo mentioned a 'special
 place' inside.

Message: 1019
%qdt:
 I found a note from =monster\_ saying that %g is expected
 at \_\_\_fakedung\_ after trying to bribe me.

Message:  1020
%qdt:
 I now have a contract with Nocturnal,
 one of the Daedric Princes of Oblivion. I have agreed
 to slay the \_monster\_ =monster\_ in %g3 stronghold of
 \_\_\_mondung\_ in exchange for Nocturnal's
 artifact, the \_artifact\_. I must have the deed
 done and meet her agent \_qgfriend\_
 in \_\_qgfriend\_ of \_\_\_qgfriend\_
 in =1stparton\_ days or I will have failed the contract.
 This may mean something worse than just me not getting
 the \_artifact\_.

Message:  1021
%qdt:
 =monster\_ offered me \_bow\_ in exchange
 for %g3 life, and I could not resist.
 %g said that it is somewhere in
 \_\_\_bowdung\_.  Since I have already
 betrayed Nocturnal, I may as well
 try to find it.

Message:  1022
%qdt:
 =monster\_ offered me the \_ring\_ in exchange
 for %g3 life, and I could not resist.
 %g said that it is somewhere in
 \_\_\_ringdung\_.  Since I have already
 betrayed Nocturnal, I may as well
 try to find it.

Message:  1023
%qdt:
 =monster\_ offered me the \_staff\_ in exchange
 for %g3 life, and I could not resist.
 %g said that it is somewhere in
 \_staffdung\_.  Since I have already
 betrayed Nocturnal, I may as well
 try to find it.

Message:  1030
<ce>                   Wait!  I know that Nocturnal has
<ce>                  sent you, and that she has promised
<ce>                   you the \_artifact\_.  But if it is
<ce>                  truly power you seek, I can top her
<ce>                   offer -- if you cease your attack,
<ce>                  I will tell you where to find \_bow\_.
<ce>                       Do you agree to my offer?

Message:  1031
<ce>                   Wait!  I know that Nocturnal has
<ce>                  sent you, and that she has promised
<ce>                   you the \_artifact\_.  But if it is
<ce>                  truly power you seek, I can top her
<ce>                   offer -- if you cease your attack,
<ce>               I will tell you where to find the \_ring\_.
<ce>                       Do you agree to my offer?

Message:  1032
<ce>                   Wait!  I know that Nocturnal has
<ce>                  sent you, and that she has promised
<ce>                   you the \_artifact\_.  But if it is
<ce>                  truly power you seek, I can top her
<ce>                   offer -- if you cease your attack,
<ce>               I will tell you where to find the \_staff\_.
<ce>                       Do you agree to my offer?

Message:  1035
<ce>                       You have slain =monster\_.
<ce>                       Prince Nocturnal will now
<ce>                              rest easier.

Message:  1040
<ce>                      Excellent.  I knew you had
<ce>                      a mercenary's heart.  \_bow\_
<ce>                      lies hidden in \_\_\_bowdung\_.
<ce>                      You have only to find it and
<ce>                       you will be a power to be
<ce>                      reckoned with indeed.  Now,
<ce>                      I bid you good day, lest you
<ce>                       decide to claim both \_bow\_
<ce>                         and the \_artifact\_ by
<ce>                     killing me.  A mercenary heart
<ce>                      such as yours should not be
<ce>                            trusted too far!

Message:  1041
<ce>                      Excellent.  I knew you had
<ce>                    a mercenary's heart.  The \_ring\_
<ce>                      lies hidden in \_\_\_ringdung\_.
<ce>                      You have only to find it and
<ce>                       you will be a power to be
<ce>                      reckoned with indeed.  Now,
<ce>                      I bid you good day, lest you
<ce>                    decide to claim both the \_ring\_
<ce>                         and the \_artifact\_ by
<ce>                     killing me.  A mercenary heart
<ce>                      such as yours should not be
<ce>                            trusted too far!

Message:  1042
<ce>                      Excellent.  I knew you had
<ce>                    a mercenary's heart.  The \_staff\_
<ce>                      lies hidden in \_staffdung\_.
<ce>                      You have only to find it and
<ce>                       you will be a power to be
<ce>                      reckoned with indeed.  Now,
<ce>                      I bid you good day, lest you
<ce>                    decide to claim both the \_staff\_
<ce>                         and the \_artifact\_ by
<ce>                     killing me.  A mercenary heart
<ce>                      such as yours should not be
<ce>                            trusted too far!

QuestTimeLapse:  \[1045\]
<ce>                 Mocking laughter echoes in your mind,
<ce>                                    
<ce>                   "You should have known better than
<ce>                    to trust me, %pcn.  Perhaps this
<ce>                    shadow of power can satisfy your
<ce>                     craving for a time, while you
<ce>                     rage vainly against those who
<ce>                 actually possess it.  Me for example.
<ce>                   I do hope sweet Nocturnal won't be
<ce>                  too displeased with your treachery."
<ce>                                    
<ce>                  \_bow\_ crumbles to dust in your hand.

Message:  1046
<ce>                 Mocking laughter echoes in your mind,
<ce>                                    
<ce>                   "You should have known better than
<ce>                    to trust me, %pcn.  Perhaps this
<ce>                    shadow of power can satisfy your
<ce>                     craving for a time, while you
<ce>                     rage vainly against those who
<ce>                 actually possess it.  Me for example.
<ce>                   I do hope sweet Nocturnal won't be
<ce>                  too displeased with your treachery."
<ce>                                    
<ce>               The \_ring\_ crumbles to dust in your hand.

Message:  1047
<ce>                 Mocking laughter echoes in your mind,
<ce>                                    
<ce>                   "You should have known better than
<ce>                    to trust me, %pcn.  Perhaps this
<ce>                    shadow of power can satisfy your
<ce>                     craving for a time, while you
<ce>                     rage vainly against those who
<ce>                 actually possess it.  Me for example.
<ce>                   I do hope sweet Nocturnal won't be
<ce>                  too displeased with your treachery."
<ce>                                    
<ce>               The \_staff\_ crumbles to dust in your hand.

Message:  1050
<ce>                 Nocturnal was most impressed at your
<ce>                     daring, %pcn.  She asked me to
<ce>                    kill you quickly as a reward for
<ce>                       such audacious treachery.

Qbn:

Item \_artifact\_ artifact Skeletons\_Key used 1014
Item \_bow\_      artifact Auriels\_Bow
Item \_ring\_     artifact Warlocks\_Ring
Item \_staff\_    artifact Staff\_of\_Magnus
Item \_letter\_ letter used 1016

Questor \_questgiver\_ pic 0x70 anyInfo 1013
Person \_qgfriend\_ group Knightly\_Order anyInfo 1011 rumors 1012

Place \_mondung\_ remote dungeon
Place \_bowdung\_ remote dungeon
Place \_ringdung\_ remote dungeon
Place \_staffdung\_ remote dungeon
Place \_fakedung\_ remote dungeon

Clock \_1stparton\_
Clock delay 1:0
Clock plague 1.0:0 2.18:40 flag 1
Clock betrayQuestor 0:2 0:30

Foe \_monster\_ is mage
Foe \_daedra\_ is Frost\_daedra

--  Quest startup:
    start timer \_1stparton\_
    log 1020 step 0
    reveal \_mondung\_
    place \_monster\_ at \_mondung\_
    have \_bow\_ set priorBow
    have \_ring\_ set priorRing
    \_letter\_ used do readLetter

--  Give him a hint on his first visit to the \_monster\_'s dungeon.
--  Once he's been bribed or killed it, be silent.

atDung when pc at \_mondung\_

giveHint task:
    when atDung and not mondead and not bribe
    say 1017
    log 1018 step 1

--  Once the PC reads the clue about the alternate hide-out, mark it on
--  the overhead map.

readLetter task:
    reveal \_fakedung\_
    log 1019 step 1

\_1stparton\_ task:
    end quest

npcclicked task:
    clicked \_qgfriend\_

mondead task:
    killed \_monster\_
    say 1035

pcgetsgold task:
    when npcclicked and mondead

--  The difference between 'unset' and 'clear' is that 'clear' would allow
--  repeated clicks of the npc (and thus repeated artifact gifts).  Unset
--  acts as a perpetual clear so that future clicks are ignored, effectly
--  dropping the npcclicked task on the floor.

    unset npcclicked
    give pc \_artifact\_
    start timer delay

injury task:
    injured \_monster\_
    place \_letter\_ at \_mondung\_

variable priorBow
variable priorRing
betrayQuestor task

--  Case selection for the various artifact bribes once the PC acquires one.
--  Take care to quash the marker that used to check for prior possession
--  of the artifact, since acquiring the artifact will perforce make it
--  true now, which is not what it is suppose to indicate.  (Note there is
--  no path to this task if the PC possesses the artifact prior to starting
--  this quest, at least assuming this is the only quest that bestows the
--  the Staff of Magnus, which is probably false, but what the original
--  quest author assumed)

findBow task:
    clicked \_bow\_
    clear priorBow

findRing task:
    clicked \_ring\_
    clear priorRing

findStaff task:
    clicked \_staff\_

--  Case selection for accepting one of the artifact bribes from \_monster\_.

betrayBow task:
    when bribe and bribeBow
    say 1040
    log 1021 step 2
    reveal \_bowdung\_
    place \_bow\_ at \_bowdung\_

betrayRing task:
    when bribe and bribeRing
    say 1041
    log 1022 step 2
    reveal \_ringdung\_
    place \_ring\_ at \_ringdung\_

betrayStaff task:
    when bribe and bribeStaff
    say 1042
    log 1023 step 2
    reveal \_staffdung\_
    place \_staff\_ at \_staffdung\_

--  Once the PC injures the \_monster\_, offer the PC a bribe of another
--  artifact to desist.  In its haste to get away from the PC, \_monster\_
--  drops a scrap of paper that the PC may read.

bribe task:
    repute \_questgiver\_ is -20
    start timer plague
    place \_monster\_ at \_fakedung\_
    place \_letter\_ at \_mondung\_
--  This command doesn't appear to have any effect here.
    rumor mill 1015
    add dialog for location \_fakedung\_

noBribe task:

--  Case selection to offer an alternative artifact as a bribe to make the
--  PC desist.  If the PC doesn't already have Auriel's Bow, offer it.
--  Else offer the Warlock's Ring if the PC already has the bow.  And
--  offer the Staff of Magnus if the PC already has both the bow and ring.
--  Unless the user has installed the CompUSA quest pack, we're guaranteed
--  that the PC doesn't already have the Staff.

bribeBow task:
    when injury and not priorBow
    prompt 1030 yes bribe no noBribe

bribeRing task:
    when injury and priorBow and not priorRing
    prompt 1031 yes bribe no noBribe

bribeStaff task:
    when injury and priorRing and priorBow
    prompt 1032 yes bribe no noBribe

--  Nocturnal understandably gets a bit anxious if the PC accepts the
--  bribe, even if he ultimately kills \_monster\_.  The frost daedra
--  won't pose a big threat except to very junior PCs, and even so,
--  the generator often fails to spawn any of them  (I wonder why.  I've
--  encountered \_daedra\_ only once during testing.  And then only because
--  I deliberately slept in an inn as soon as the plague task had
--  triggered)

plague task:
    create foe \_daedra\_ every 3 minutes 4 times with 100% success

boast task:
    injured \_daedra\_
    say 1050

--  But, if the PC takes possession of the bribe before dispatching \_monster\_
--  Nocturnal will acknowledge the deception by destroying the artifact bribe.
--  If the PC does dispatch \_monster\_ before the timers run out, he can
--  recover the destroyed artifact from the \_fakedung\_.

byeBow task:
    when betrayQuestor and not priorBow
    place \_bow\_ at \_fakedung\_
    say 1045

byeRing task:
    when betrayQuestor and priorBow and not priorRing
    place \_ring\_ at \_fakedung\_
    say 1046

byeStaff task:
    when betrayQuestor and priorBow and priorRing
    place \_staff\_ at \_fakedung\_
    say 1047

--  Case selection to detect (for Nocturnal) that the PC has apparently
--  honored the \_nonster\_'s bribe.

betrayedForBow task:
    when findBow and not mondead
    start timer betrayQuestor

betrayedForRing task:
    when findRing and not mondead
    start timer betrayQuestor

betrayedForStaff task:
    when findStaff and not mondead
    start timer betrayQuestor

--  On the other hand, if the PC has first dispatched \_monster\_ for Nocturnal,
--  allow the PC to garner the artifact bribe before collecting the Key
--  from Nocturnal's slavey.

saveBow task:
    when findBow and mondead
    make \_bow\_ permanent

saveRing task:
    when findRing and mondead
    make \_ring\_ permanent

saveStaff task:
    when findStaff and mondead
    make \_staff\_ permanent

--  Once the Key is collected, allow the PC to bask in the post-quest
--  rumors of success for a bit before finally ending the quest.

delay task:
    end quest
