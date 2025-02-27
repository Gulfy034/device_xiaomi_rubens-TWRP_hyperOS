### TWRP device tree for Redmi K50 (rubens)

EN/[CN](README_zh-CN.md)

=========================================

The Redmi K50 (codenamed _"rubens"_) is a high-end, mid-range smartphone from Xiaomi.

> [!TIP]
> This is for the **STABLE** version of the redmi k50 (rubens) device.

> Only uses in **CN** **STABLE** **HyperOS**, do not build it for MIUI !

## Device specifications

Basic   | Spec Sheet
-------:|:-------------------------
CPU     | Octa-core CPU with 4x Arm Cortex-A78 up to 2.85GHz
Chipset | Mediatek Dimensity 8100
GPU     | Mali-G610 MC6
Memory  | 8/12 GB RAM (LPDDR5 6400Mbps)
Shipped Android Version | 12
Storage | 128/256/512 GB (UFS 3.1)
Battery | Li-Po 5500 mAh, non-removable
Display | 1440 x 3200 pixels, 6.67 inches, 60/120 hz

![Redmi K50](https://cdn.cnbj0.fds.api.mi-img.com/b2c-shopapi-pms/pms_1653381863.47942179.png)

## Tips:

> is fixing vendor mounting

> is fixing userdata decrypting

## Features

Works:

- [X] ADB
- [ ] Decryption (TODO: userdata FBE decryption)
- [X] Display
- [X] Fasbootd
- [X] Flashing
- [X] MTP
- [X] Sideload
- [X] USB OTG
- [X] Vibrator

## Compile

First checkout [minimal twrp with aosp tree](https://github.com/minimal-manifest-twrp):

```
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1
repo sync -j$(nproc --all)
```

Then add these projects to .repo/manifest.xml:

```xml
<project path="device/xiaomi/rubens" name="D8100-9000-TWRP-Device-Tree/device_xiaomi_rubens-TWRP" remote="github" revision="twrp-13" />
```

Then clone this repository and rename the folder into "rubens" 

move this twrp repository to `/sync/path/to/device/xiaomi`

Finally execute these:

```
source build/envsetup.sh
repopick <needed patch>
lunch twrp_rubens-eng
```
## To use it:

> [!WARNING]
> Because of the MTK chipset, you MUST flash the twrp in vendor_boot_a partication.

```
fastboot getvar current_slot  //make sure you have sellect the correct slot
fastboot flash vendor_boot_a out/target/product/rubens/vendor_boot.img
```