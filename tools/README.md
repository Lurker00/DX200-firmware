# Rockchip FactoryTool

[**`Rockchip_FactoryTool_v1.39.zip`**](https://github.com/Lurker00/DX200-firmware/raw/master/tools/Rockchip_FactoryTool_v1.39.zip) is the same tool that is distributed by iBasso, with only change that it starts in English without additional efforts.

Please read the instructions inside the zip archive on how to use it.

# Tracks to test bit-perfect playback
## DTS-encoded PCM

**Warning:** *Do not try to listen these audio files in headphones! Always mute receiver output or put the volume to a very low level! Being unproperly decoded, these sound files produce high volume noise.*

A PCM sound stream may contain [DTS-encoded data](https://en.wikipedia.org/wiki/DTS_(sound_system)) for 5.1 channel sound. Such a stream can be played back via S/PDIF output, connected to a DTS decoder (e.g. any modern Home Theatre receiver). For player application and all the subsequent path, such a stream looks like a usual PCM stream. DTS decoder is able to lock and decode DTS only if it receives the stream untouched. It means, if DTS decoder locks and decode the stream, the stream is passed bit perfect from the file to the decoder.

For a test, download a track, unzip. Connect your device' S/PDIF output to S/PDIF input of a DTS-capable decoder, and start playing the track in any software player. On some devices or with some software players, you may need to adjust the volume to the maximum, to prevent PCM stream recalculation (software volume control). In case of success, the decoder should display an indicator that DTS stream is detected. Then you'll hear a good sound. In case of a failure, there will be no indication, and you'll hear very unpleasant digital noise.

* [**dts_test44.zip**](https://github.com/Lurker00/DX200-firmware/raw/master/tools/dts_test44.zip) - DTS track in 16/44.1KHz format.
* [**dts_test48.zip**](https://github.com/Lurker00/DX200-firmware/raw/master/tools/dts_test48.zip) - DTS track in 16/48KHz format.

With iBasso DX200 in full Android mode, MangoPlayer passes both tests. Other players (tried USB Audio Player PRO, HibyMusic, Neutron Music Player) pass only 16/44.1 test, which means that Android sound path is locked to 44.1kHz sampling rate. With [USB Audio](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md) software, all three players pass both tests.

**Edit:** The latest release of Neutron Music, with support of DX200 extension in its Generic Driver, passes all the tests!

## DSD64 in DoP

DSD encoded in DoP looks like PCM for any music player. Being played using a DAC which can't decode DSD, it produces very quiet sound, almost silence. But, with DSD DoP capable DAC, you can hear the music. Here is a file with 1 minute sample:

* [**DSD64-in-DoP.flac**](https://github.com/Lurker00/DX200-firmware/raw/master/tools/DSD64-in-DoP.flac) - DSD64 in DoP (PCM 24/176.4kHz).

If a software player modifies PCM stream in either way, the output stream looses the information that it is DoP-encoded, and the DAC does not produce the music.

In normal mode, XMOS chip in DX200 does not decode DoP. With [firmware 2.5.141L1](https://github.com/Lurker00/DX200-firmware/releases/latest), my build of libtinyalsa.so switches XMOS chip to the mode in which it detects and decoded DoP automatically. It switches XMOS chip into this mode for recordings with sample rate above 192kHz, and for 176.4kHz, because for DSD64, DoP is essencially PCM 24/176.4.

With [firmware 2.5.141L1](https://github.com/Lurker00/DX200-firmware/releases/latest), both Mango Player under Android and Neutron Music with Generic Driver *and* disabled EQ, filters and other enchacements, produce music.

With USB Audio, all the apps that support it (USB Audio Player PRO, HibyMusic, Neutron Music Player) also produce music. It means, they all are capable of bit perfect playback.
