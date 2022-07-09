# Sound And Movies

Media files can be easily imported as loose files from the _StreamingAssets_ folder with predefined formats and settings. An alternative is to bundle data inside a mod to benefit of load order and additional customization.


## Sound Effects

Sound effects for player, enemies and environment can be imported as loose files with **wav** extension or bundled as instances of **AudioClip**. The [AudioClip](https://docs.unity3d.com/Manual/class-AudioClip.html) type allow to set some options that affects quality and size of the sound file. _**Decompress On Load**_ should be a good choice as _**Load Type**_ for this kind of sound data.

* _StreamingAssets/Sound/_**SoundClipName.wav**  
    ex: StreamingAssets/Sound/PlayerFootstepNormal.wav
* **SoundClipName** inside a mod as an AudioClip with any supported format.

A complete list of sounds names can be found [here](https://raw.githubusercontent.com/Interkarma/daggerfall-unity/master/Assets/Scripts/SoundClips.cs).


## Songs

Songs can be imported as MIDI data or digital sound data. Digital files always take precedence over MIDI equivalents.


### MIDI

MIDI files can be imported as loose files or bundled as instances of [TextAsset](https://docs.unity3d.com/Manual/class-TextAsset.html).

*   _StreamingAssets/Sound/_**Song\_Name.mid**  
    ex: StreamingAssets/Sound/song\_magic\_2.mid
*   **Song\_Name** inside a mod as a TextAsset with extension **.mid.bytes**


### Digital

Digital sound files can be imported as loose files with **ogg** extension or bundled as instances of **AudioClip**. The [AudioClip](https://docs.unity3d.com/Manual/class-AudioClip.html) type allow to set some options that affects quality and size of the sound file. **Streaming** should be a good choice as _**Load Type**_ for this kind of sound data.

* _StreamingAssets/Sound/_**Song\_Name.ogg**  
    ex: StreamingAssets/Sound/song\_magic\_2.ogg
* **Song\_Name** inside a mod as an AudioClip with any supported format.

A complete list of songs names can be found [here](https://raw.githubusercontent.com/Interkarma/daggerfall-unity/master/Assets/Scripts/SongFiles.cs). Run **tdbg** via game console to check current playing song.


## Movies

Movies can be imported as loose files with **mp4** extension (**webm** on Linux) or bundled as instances of [VideoClip](https://docs.unity3d.com/Manual/class-VideoClip.html).

* _StreamingAssets/Movies/_**name.mp4**  
    ex: StreamingAssets/Movies/ANIM0001.mp4
* **name** inside a mod as a VideoClip with any supported format.

Names: All ANIM00\*\*.VID files (from 00 to 15) plus DAG2.VID.


## Tools

* **[DFJukebox](http://www.dfworkshop.net/downloads/old-tools/daggerfall-jukebox/)**: Play audio directly from within Daggerfall Jukebox, or export to standard file formats. Sound is exported to .wav, and music to .mid.
* **DaggerSound**: Extract sound files from DAGGER.SND. You may need to use Audacity’s raw import to actually make a use of the extracted .wav files (see [here](http://xlengine.com/forums/viewtopic.php?f=14&t=423&start=50)).
* **WINRipper**: Extract HMI files from MIDI.BSA and convert them to MIDI files.
* **dfmusics2.zip**: All MIDI song files from the game.

* **VIDTitle**: A collection of tools for adding subtitles, captions, or other overlays to Daggerfall VID files.
* **[DagVid V1.01](https://web.archive.org/web/20140329212711/http://svatopluk.com/daggerfall/download/dagvid.stm)**: A Daggerfall and Skynet video file player for Win 9X/NT, with source code (broken download link).
* **[DFVid2AVI 0.43b](https://web.archive.org/web/20140329223214/http://svatopluk.com/daggerfall/download/vid2avi.stm)**: Write out to AVI.

You can get the tools without a link from the [UESP wiki](https://www.uesp.net/wiki/Daggerfall:Files).
