# Rockchip factoryTool

[**`Rockchip_FactoryTool_v1.39.zip`**](https://github.com/Lurker00/DX200-firmware/raw/master/tools/Rockchip_FactoryTool_v1.39.zip) is the same tool that is distributed by iBasso, with only change that it starts in English without additional efforts.

Please read the instructions inside the zip archive on how to use it.

# Tracks to test bit-perfect playback

**Warning:** *Do not try to listen these audio files in headphones! Always mute receiver output or put the volume to a very low level! Being unproperly decoded, these sound files produce high volume noise.*
A PCM sound stream may contain [DTS-encoded data](https://en.wikipedia.org/wiki/DTS_(sound_system)) for 5.1 channel sound. Such a stream can be played back to an S/PDIF output, connected to a DTS decoder (e.g. any modern Home Theatre receiver). For player application and all the subsequent path, such a stream looks like a usual PCM stream. DTS decoder is able to lock and decode DTS only if it receives the stream untouched.

For a test, download a track, unzip. Connect your device' S/PDIF output to S/PDIF input of a DTS-capable decoder, and start playing the track in any software player. On some devices or with some software players, you may need to adjust the volume to the maximum, to prevent PCM stream recalculation (software volume control). In case of success, the decoder should display an indicator that DTS stream is detected. Then you'll hear a good sound. In case of a failure, there will be no indication, and you'll hear very unpleasant digiatl noise.
