# iBasso DX200 firmware modified by Lurker

**DISCLAIMER:** No changes were made to the basic player functionality and behavior! Expect the same bugs and misbehavior found in the base stock firmware! **No warranty at all: use the modified firmware at your own risk and responsibility!**

To download the latest releases, please go to the [Releases section](https://github.com/Lurker00/DX200-firmware/releases).

To check the current version in Android mode, go to _Settings_, _About DX200_, _Build number_ at the bottom. In Mango mode, open _Settings_, _Advanced_, _System Info_, and look to _Firmware version_ and suffix (e.g. L0) in _Model number_.

To flash the firmware, you need
* a computer running Windows XP or later, and [Rockchip FactoryTool v1.39](https://github.com/Lurker00/DX200-firmware/tree/master/tools), or
* Mac/Linux with rkflashtool version 6.1 or later. See [below for details](#flash-dx200-firmware-using-rkflashtool-maclinux).

Flashing this firmware will not clean the user data. To return to the official firmware, just flash it per its instruction. You may only need to do a factory reset to get rid of icons of software, pre-installed in my firmware builds, but sometimes it's enough to delete them a usual way.

1. [Changes made](#changes-made)
1. [Use of USB Reader in Mango to access microSD card](#use-of-usb-reader-in-mango-to-access-microsd-card)
1. [Flash DX200 firmware using rkflashtool (Mac/Linux)](#Flash-dx200-firmware-using-rkflashtool-maclinux)

## Changes made
### Android mode
* Modified **Android's `DeviceIdleController`** (2.7.188L0+): with the official firmware, Android never goes into [Doze mode!](https://developer.android.com/training/monitoring-device-state/doze-standby.html#understand_doze). This is because the original [`DeviceIdleController` has turned off normal execution of device idling](https://android.googlesource.com/platform/frameworks/base/+/marshmallow-release/services/core/java/com/android/server/DeviceIdleController.java#1413) for devices without significant motion sensor. The modified version has removed any motion detectors, and the default `DeviceIdleController` constants are tuned for DAP use. See also [***System settings***](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md#system-settings) of USB Audio app to improve energy saving even more.<br />
* **Music detector**: it keeps Android in active state while music is playing, and prevents deep sleep for some minutes after the music has been stopped, to allow to resume playback by only pushing play/pause button. The default period is 5 minutes. You can change this value in the [***Settings***](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md#settings), ***Idle Timeout***. You don't need to keep USB Audio running for this feature.
* **USB Mass Storage** (2.2.110-L1+) to direct access of your microSD card when you connect DX200 to a computer. It replaces MIDI choice in the pull-down menu of USB connections. When selected, SD card is unmounted internally, and is not visible if you return back to MTP. It is mounted back right after disconnect from the computer.
* Embedded [**USB Audio for DX200**](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md) application with advanced features, possible only for a built-in app (2.2.110+). In its [***System settings***](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md#system-settings) (2.7.188L0+) you can disable Google services and Media scanner, and turn Android's Battery saver permanently on, to reduce background activity almost to zero level.
* Embedded a special build of [**HibyMusic**](https://play.google.com/store/apps/details?id=com.hiby.music), compatible with USB Audio. All the required changes made by me, with kind permission of [HiBy Music](http://www.hiby.cd/index_en.aspx). This build may co-exist with official releases installed from Google PlayMarket.
* Added **Google Play Market**. It is not the latest version available, but it corresponds to the pre-installed Google Play Services. Can be disabled and enabled back via [***System settings***](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md#system-settings) of USB Audio app, along with Google Services (2.7.188L0+).
* Added **SuperSU** for those who need root access.
* Added support to mount exFAT and NTFS file systems by UUID, not as `default` (2.2.110-L2+). This allows to use microSD and a USB OTG disk at the same time, without problems. 2.2.125-L1 has restored support for badly formatted VFAT SD cards.
* Removed `rild` service (2.2.110L0+) and disabled any telephony and SMS services (2.3.125L2+).
* Battery level percentage indicator is back, and volume level indicator is visible at the lock screen (2.3.125-L1+).
* A custom build of `libtinyalsa.so` to workaround a bug in Android, that caused `mediaserver` to crash (2.3.125-L1+). This problem was not visible for most users, because crashes and restarts happened on the background, between audio playbacks.<br /> Starting from 2.5.141L1, it allows [Neutron Music](https://play.google.com/store/apps/details?id=com.neutroncode.mp) to play DSD in DoP, and PCM up to 32/384kHz, transparently, without additional efforts. From 2.7.188L1, Native DSD playback up to DSD512 is supported for Neutron version 1.98.1 and higher.
* Custom builds of Android Power, Lights and Vibrator [HAL](https://source.android.com/reference/hal/)s (2.3.125-L2+).
* Workaround for confusing and annoying "Choose app for the USB device" (or similar) Android prompt when Mango plays DSD, or USB Audio turns the interface on (2.5.141L1+).
* Turned off system logs, to decrease unwanted CPU load and background activity. Can be turned back on via [***System settings***](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md#system-settings) (2.7.188L0+).<br />
**Note**: by default, logs are disabled only for 32-bit applications, because disabling them for 64-bit apps creates problems for SuperSU user interface (only user interface, not for its basic functionality!). They can be completely turned off via [***System settings***](https://github.com/Lurker00/DX200-USB-Audio-Release/blob/master/README.md#system-settings), and turned on, if required.

Custom build of Android Power HAL module has implemented the following strategies for 8 CPU cores:
* Screen on: 8 in normal, 4 in Battery Saving modes.
* Screen off: 4 when music is playing (by any application!), 1 when idle (including USB DAC mode).

Custom build of Android Lights HAL module has implemented fast control of backlight, and removed control of LEDs, which are missing in DX200 anyway.<br />
Custom build of Android Vibrator HAL module just does nothing, because there is no corresponding hardware in DX200.

### Mango mode
* Removed Android services, that are not used in this mode.
* Enabled USB Reader mode ([read below](https://github.com/Lurker00/DX200-firmware/blob/master/README.md#use-of-usb-reader-in-mango-to-access-microsd-card)).
* Fonts are replaced with a font based on `Roboto Condensed` and `Arial Unicode MS`.
* Removed video codecs from Mango multimedia engine.
* Turned off system logs, to decrease unwanted CPU load and background activity.

## Use of USB Reader in Mango to access microSD card
Write access to microSD card using USB Reader mode of Mango may lead to corrupted files and the file system! The reason is that Mango does not unmount the SD card internally, so that an access possible from both sides (DX200 and PC/Mac) at the same time, without knowing of each other.

If you really want to use this feature, it is strongly recommended to perform the following steps in this particular order:
1. Connect DX200 to PC/Mac as Card Reader right after device boot.
2. Check the flash card for corrupted file system. Most probably at least the volume is not closed.
3. Copy your files to or from the SD card.
3. Safely disconnect the device from PC/Mac.
4. Turn DX200 off, then on, and only then make operations that may need write access to the SD card.

## Flash DX200 firmware using rkflashtool (Mac/Linux)
You need to have installed [rkflashtool version 6.1 or later](https://sourceforge.net/projects/rkflashtool/files/). For Lunux, you have to build it from the source code. For MacOS, you can build it yourself as well, but there are pre-built binaries, also in [Homebrew](http://brewformulas.org/Rkflashtool).

Then, you need to download a zip archive with "-rkflashtool" suffix, which contains two files: `boot.img` and `system.img`, and unzip it. If you have downloaded and unzipped it into your `Download` folder, the path is `/Users/username/Downloads` with your actual user name.

### Install Homebrew and rkflashtool
These steps you need to perform only once, to set up the required environment. Open **Terminal** from your Mac Utility folder. Copy and paste the following line into the command prompt and hit Enter:

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null

When you have Homebrew installed, copy and paste the following command and hit Enter:

    brew install rkflashtool

### Prepare DX200 for firmware loading
1. Turn DX200 off.
2. Press and hold Pause/Play button.
3. Connect DX200 to your computer via USB.
4. In a five to ten seconds, release Play/Pause.

Now DX200 should be in factory flash mode. Then type the following commands (possibly with or in su):

    rkflashtool v
    rkflashtool n

to check if `rkflashtool` sees and recognizes your DX200.

### Flash the firmware
Change the current working directory in **Terminal** to one where you have unzipped firmware files, e.g.

    cd /Users/username/Downloads

with your actual user name and the path. Then type in the directory where you have unzipped firmware files:

    rkflashtool w boot < boot.img
    rkflashtool w system < system.img

At the end of the each process you'll see a message `premature end-of-file reached`: it is normal, because files are smaller than flash partition sizes. Now you can issue

    rkflashtool b

to reboot the device, or do it manually.

The Mac specific steps above were derived from a [forum post by Likeimthere](https://www.head-fi.org/threads/791531/page-501#post-13649223).
