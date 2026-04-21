# TWRP device tree for Lenovo Legion Y700 gen3 (2025) (TB321FU)

TWRP build for Legion Tablet Y700 gen3 (2025).

## Other devices
- [Y700 gen2 (2023)](https://github.com/polygraphene/android_device_lenovo_TB320FC)
- [Y700 gen4](https://github.com/polygraphene/android_device_lenovo_TB322FC)

## Flash instructions
1. Download recovery image from [release](https://github.com/polygraphene/android_device_lenovo_TB321FU/releases).
2. Unlock bootloader
3. Flash recovery
   - Launch bootloader then run the following command.
   ```sh
   fastboot flash recovery twrp-downloaded-file-name.img
   ```

## Supported features

Blocking checks
- [x] Correct screen/recovery size
- [x] Working Touch, screen
- [x] Backup to internal/microSD
- [x] Restore from internal/microSD
- [x] reboot to system
- [x] ADB

Medium checks
- [x] update.zip sideload
- [x] UI colors (red/blue inversions)
- [x] Screen goes off and on
- [x] F2FS/EXT4 Support, exFAT/NTFS where supported
- [x] all important partitions listed in mount/backup lists
- [ ] backup/restore to/from external (USB-OTG) storage (not supported by the device)
- [ ] backup/restore to/from adb (https://gerrit.omnirom.org/#/c/15943/)
- [x] decrypt /data
- [x] Correct date

Minor checks
- [x] MTP export
- [x] reboot to bootloader
- [x] reboot to recovery
- [ ] poweroff
- [x] battery level
- [x] temperature
- [ ] encrypted backups
- [ ] input devices via USB (USB-OTG) - keyboard, mouse and disks (not supported by the device)
- [ ] USB mass storage export
- [x] set brightness
- [ ] vibrate
- [ ] screenshot
- [ ] partition SD card

## Note
1. Based on TWRP 3.7.1
2. Includes following patches
   - Add gatekeeper and boot AIDL support for /data decrytion.
        - https://github.com/polygraphene/android_hardware_interfaces/tree/android-12.1-TB321FU
   - Fix for a graphical glitch
        - https://github.com/polygraphene/android_bootable_recovery/commit/4e4dd385974e275fac5f894bf7ad00fb17004e62
   - Fix for landscape theme
        - https://github.com/polygraphene/android_device_lenovo_TB320FC/pull/2#issuecomment-2525420584
   - Support for work profile decryption
        - https://github.com/TeamWin/Team-Win-Recovery-Project/issues/1256#issuecomment-2414079092

## Device specifications

Component              | Model
----------------------:|:-------------------------
SoC                    | Qualcomm SM8650-AB Snapdragon 8 Gen 3 (4 nm)
CPU                    | Octa-core (1x3.3 GHz Cortex-X4 & 3x3.2 GHz Cortex-A720 & 2x3.0 GHz Cortex-A720 & 2x2.3 GHz Cortex-A520)
GPU                    | Adreno 750
Memory                 | 12GB / 16GB
Storage                | UFS 4.0 256GB / 512GB
Battery                | Li-Po 6550 mAh
Display                | 1600 x 2560 pixels (IPS LCD, 165Hz, 8.8 inches)
Release                | 2024, October

## To build 
```bash
# Fetch sources
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp -b twrp-12.1
repo sync -j 20 --force-sync

# Apply patches
(cd bootable/recovery; git fetch https://github.com/polygraphene/android_bootable_recovery android-12.1-TB321FU && git checkout FETCH_HEAD)
(cd frameworks/native; git fetch https://github.com/polygraphene/android_frameworks_native android-12.1-TB321FU && git checkout FETCH_HEAD)
(cd hardware/interfaces; git fetch https://github.com/polygraphene/android_hardware_interfaces android-12.1-TB321FU && git checkout FETCH_HEAD)
(cd system/core; git fetch https://github.com/polygraphene/android_system_core android-12.1-TB321FU && git checkout FETCH_HEAD)
(cd system/extras; git fetch https://github.com/polygraphene/android_system_extras android-12.1-TB321FU && git checkout FETCH_HEAD)
(cd system/tools/aidl; git fetch https://github.com/polygraphene/android_system_tools_aidl android-12.1-TB321FU && git checkout FETCH_HEAD)
(cd system/vold; git fetch https://github.com/polygraphene/android_system_vold android-12.1-TB321FU && git checkout FETCH_HEAD)

# Build
export ALLOW_MISSING_DEPENDENCIES=true
. build/envsetup.sh
lunch twrp_TB321FU-eng
mka recoveryimage
```
