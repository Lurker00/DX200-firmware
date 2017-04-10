# iBasso DX200 firmware modified by Lurker

**DISCLAIMER:** No changes were made to the basic player functionality and behavior! Expect the same bugs and misbehavior found in the base stock firmware! **No warranty at all: use the modified firmware at your own risk and responsibility!**

To download the latest releases, please go to the [Releases section](https://github.com/Lurker00/DX200-firmware/releases).

To check the current version in Android mode, go to _Settings_, _About DX200_, _Build number_ at the bottom. In Mango mode, open _Settings_, _Advanced_, _System Info_, and look to _Firmware version_ and suffix (e.g. L0) in _Model number_.

To flash the firmware, you need a computer running Windows XP or later, and [Rockchip FactoryTool v1.39](https://github.com/Lurker00/DX200-firmware/tree/master/tools). Flashing this firmware will not clean the user data.

## Use of USB Reader in Mango to access microSD card
In firmware 2.1.94, write access to microSD card using USB Reader mode of Mango may lead to corrupted files and the file system! The reason is that Mango does not unmount the SD card internally, so that an access possible from both sides (DX200 and PC/Mac) at the same time, without knowing of each other.

If you really want to use this feature, it is strongly recommended to perform the following steps in this particular order:
1. Connect DX200 to PC/Mac as Card Reader right after device boot.
2. Check the flash card for corrupted file system. Most probably at least the volume is not closed.
3. Copy your files to or from the SD card.
3. Safely disconnect the device from PC/Mac.
4. Turn DX200 off, then on, and only then make operations that may need write access to the SD card.

## Changes made
### Mango mode
* Removed Android services, that are not used in this mode.
* Enabled USB Reader mode (see above).
* Fonts are replaced with a font based on `Roboto Condensed` and `Arial Unicode MS`.
* Removed video codecs from Mango multimedia engine.

### Android mode
* Added Google Play Market. It is not the latest version available, but it corresponds to the pre-installed Google Play Services.
* Added SuperSU for those who need root access.
