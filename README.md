# iBasso DX200 firmware modified by Lurker

**DISCLAIMER:** No changes were made to the basic player functionality and behavior! Expect the same bugs and misbehavior found in the base stock firmware! **No warranty at all: use the modified firmware at your own risk and responsibility!**

To download the latest releases, please go to the [Releases section](https://github.com/Lurker00/DX200-firmware/releases).

To check the current version in Android mode, go to _Settings_, _About DX200_, _Build number_ at the bottom. In Mango mode, open _Settings_, _Advanced_, _System Info_, and look to _Firmware version_ and suffix (e.g. L0) in _Model number_.

To flash the firmware, you need
* a computer running Windows XP or later, and [Rockchip FactoryTool v1.39](https://github.com/Lurker00/DX200-firmware/tree/master/tools), or
* Mac/Linux with rkflashtool version 6.1 or later. See [below for details](#flash-dx200-firmware-using-rkflashtool).

Flashing this firmware will not clean the user data. To return to the official firmware, just flash it per its instruction. You may only need to do a factory reset to get rid of icons of software, pre-installed in my firmware builds, but sometimes it's enough to delete them a usual way.

## Changes made
### Android mode
* **USB Mass Storage** (2.2.110-L1+) to direct access of your microSD card when you connect DX200 to a computer. It replaces MIDI choice in the pull-down menu of USB connections. When selected, SD card is unmounted internally, and is not visible if you return back to MTP. It is mounted back right after disconnect from the computer.
* Embedded [**USB Audio for DX200**](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md) application with advanced features, possible only for a built-in app (2.2.110+). You may find its *CPU Turbo Mode* function useful even if you don't want to use USB Audio interface.
* Embedded a special build of [**HibyMusic**](https://play.google.com/store/apps/details?id=com.hiby.music), compatible with USB Audio. All the required changes made by me, with kind permission of [HiBy Music](http://www.hiby.cd/index_en.aspx). This build may co-exist with official releases installed from Google PlayMarket.
* Added **Google Play Market**. It is not the latest version available, but it corresponds to the pre-installed Google Play Services.
* Added **SuperSU** for those who need root access.
* Added support to mount exFAT and NTFS file systems by UUID, not as `default` (2.2.110-L2+). This allows to use microSD and a USB OTG disk at the same time, without problems. 2.2.125-L1 has restored support for badly formatted VFAT SD cards.
* Removed `rild` service. It provides an interface to the telephony part which is absent (2.2.110+).
* Battery level percentage indicator is back, and volume level indicator is visible at the lock screen (2.3.125-L1+).
* A custom build of `libtinyalsa.so` to workaround a bug in Android, that caused `mediaserver` crashes (2.3.125-L1+). This problem was not visible for most users, because crashes and restarts happened on the background, between audio playback.
* Custom builds of Android Power HAL (smart control over CPU cores for to ensure the performance when required, and to reduce power consumption when idle) and Lights HAL (fast backlight control, no attempts to control absent LEDs)(2.3.125-L2+).

Custom build of Android Power HAL module has implemented the following strategies for 8 CPU cores:
* Screen on: 8 in normal, 4 in Battery Saving modes.
* Screen off: 4 when music is playing (by any application!), 1 when idle.

### Mango mode
* Removed Android services, that are not used in this mode.
* Enabled USB Reader mode ([read below](https://github.com/Lurker00/DX200-firmware/blob/master/README.md#use-of-usb-reader-in-mango-to-access-microsd-card)).
* Fonts are replaced with a font based on `Roboto Condensed` and `Arial Unicode MS`.
* Removed video codecs from Mango multimedia engine.

## Use of USB Reader in Mango to access microSD card
Write access to microSD card using USB Reader mode of Mango may lead to corrupted files and the file system! The reason is that Mango does not unmount the SD card internally, so that an access possible from both sides (DX200 and PC/Mac) at the same time, without knowing of each other.

If you really want to use this feature, it is strongly recommended to perform the following steps in this particular order:
1. Connect DX200 to PC/Mac as Card Reader right after device boot.
2. Check the flash card for corrupted file system. Most probably at least the volume is not closed.
3. Copy your files to or from the SD card.
3. Safely disconnect the device from PC/Mac.
4. Turn DX200 off, then on, and only then make operations that may need write access to the SD card.

## Flash DX200 firmware using rkflashtool (Mac/Linux)
First of all, you need to download a zip archive with "-rkflashtool" suffix, which contains two files: `boot.img` and `system.img`. You need to install [rkflashtool version 6.1 or later](https://sourceforge.net/projects/rkflashtool/files/). For Lunux, you have to build it from the source code. For MacOS, you can build it yourself as well, but there are pre-built binaries, also in [Homebrew](http://brewformulas.org/Rkflashtool).

1. Turn DX200 off.
2. Press and hold Pause/Play button.
3. Connect DX200 to your computer via USB.
4. In a couple of seconds, release Play/Pause.

Now DX200 should be in factory flash mode. Then type the following commands (possibly with or in su):

    rkflashtool v
    rkflashtool n

to check if `rkflashtool` sees and recognises your DX200. Then type in the directory where you have unzipped firmware files:

    rkflashtool w boot < boot.img
    rkflashtool w system < system.img

At the end of the each process you'll see a message `premature end-of-file reached`: it is normal, because files are smaller than flash partition sizes. Now you can issue

    rkflashtool b

to reboot the device, or do it manually.
