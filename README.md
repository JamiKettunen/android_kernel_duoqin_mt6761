# Replacement Android kernel for Duoqin F21 Pro
Duoqin (Xiaomi) never provided source code for the original `alps-mp-r0.mp1-V7.13`-based v4.19.127
kernel so here's a best-effort reverse engineered attempt at providing a replacement for it based
on [`alps-mp-r0.mp1-V8.46`](https://github.com/JamiKettunen/android_kernel_duoqin_mt6761/tree/8f66a5f4b600)
with some additional commits on top from https://github.com/nokia-mt6761-devs/android_kernel_nokia_mt6761/tree/lineage-21-r :^)

## Known issues compared to stock kernel
- Bluetooth broken (`libbluetooth_jni.so` fails to `getprop` something and then `com.android.bluetooth` dies?)
- Internal speaker audio output (BT audio is separate and should work once that's fixed etc)
  - Internal microphone input on the other hand seems to work already
- Touchscreen
  - Seems to be `gsl3680@62` of `i2c0@11007000`, bits of original source seen on https://www.jianshu.com/p/0c2e2c9de62e
  - [`GT1151`](drivers/input/touchscreen/GT1151) enabled on stock likely unused? causes kernel panic too currently
- Cameras (missing `sp5508_mipi_raw` & `sp2507_mipi_raw` from [`drivers/misc/mediatek/imgsensor/src/common/v1`](drivers/misc/mediatek/imgsensor/src/common/v1) + device-specific integration)
  - Flash torchlight also seems tied to cameras, `mt6370_pmu_fled` itself could work already and probes fine at least
- Battery drains pretty quickly (possibly caused by some of the others above)

## What works?
Hopefully everything else, remains to be tested on other units etc :p

## Pic of a device with this kernel booted
![Duoqin F21 Pro booted using an artifacts built from this kernel source showing "cat /proc/version" output in Termux](https://i.imgur.com/sborcuc.jpeg)

## See also
- https://github.com/techyminati/android_kernel_teracube_mt6762/tree/2e-r-oss
- https://github.com/realme-mediatek-dev/android_kernel_realme_mt6765/tree/android-11.0

## Original Linux kernel README
[Click here](README)
