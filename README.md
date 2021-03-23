# macOS on Thinkpad X1 Carbon 7th Generation
OpenCore-based Hackintosh EFI and guide for Lenovo Thinkpad X1 Carbon Gen 7. This guide has been generated for both the model numbers specified below.

<p float="center">
  <img src="./docs/macOS-overview.png" width="500" />
  <img src="./docs/laptop-image.png" width="300" /> 
</p>

# Overview
## Summary

This X1C7 Hackintosh project aims to be an all-in-one maintained hub for Opencore-based hackintoshes on the Thinkad X1 Carbon Gen 7. In short, this x1c7-hackintosh is very stable and is currently our daily driver (Simon and Aidan). We fully recommend this project to anyone looking for a MacBook alternative. There are also multiple contributors to this project who have all helped in various fashions - see credits.

This repo is meant to serve as a hub and guide for users of the X1C7 and X1Cx community in general.
<br />

## Functional Overview
What works, what doesn't and why.
<details>
<summary><strong> WHAT IS WORKING </strong></summary>

### Install
| working | Device / Step                             | Comment            |
|:-------:|:------------------------------------------|:-------------------|
| ✅ | Booting macOS installer                        |                    |
| ✅ | Installed to HD                                |                    |
| ✅ | Installed to HD and Dualbooting Windows on the same drive Windows                                | Use [this](https://www.youtube.com/watch?v=ztxHRGdX0Sw&t=3s) guide to setup dualboot on the same drive|

### Post-Install

| working | Device / Step                             | Comment            |
|:-------:|:------------------------------------------|:-------------------|
| ✅ | Graphics                                       | Requires `WhateverGreen.kext` |
| ✅ | Touchpad                                       | Requires ``VoodooGPIO, VoodooI2CServices,VoodooInput`` This was very trial-and-error based and I reccomend looking at our config.plist. Order and location matter. |
| ✅ | Trackpoint                                     | Requires ``VoodooPS2`` |
| ✅ | Keyboard                                       | Requires ``VoodooPS2`` |
| ✅ | Keyboard-Multimedia Fn keys                    | Requires `YogaSMC.kext` + **TODO: Add ACPI here** and [YogaSMC-App](https://github.com/zhen-zen/YogaSMC) |
| ✅ | WiFi                                           | Native WiFi with `AirportItlwm.kext` - no companion app required |
| ✅ | Bluetooth                                      | `IntelBluetoothFirmware.kext` and `IntelBluetoothInjector.kext` <br> ⚠️ audio input (e.g. of headset) is not working, see [#3](https://github.com/aidanchandra/x1c7-hackintosh/issues/3) |
| ❌ | WWAN                                           | DISABLED at BIOS to conserve power|
| ✅ | Ethernet                                       | `IntelMausi.kext` for bundled USB-C adapter |
| ✅ | Hibernation                                    | ``hibernatemode=3`` |
| ✅ | HDMI output                                    | Requires **WEG?** |
| ✅ | USB A / USB C                                  | |
| ✅ | Thunderbolt 3                                  | Tested with a Vega 64 EGUP in a Razer Core X Enclosure, requires TB3 BIOS Assist to be ENABLED|
| ✅| Webcam                                          | _checked on 2021-02-19_ |
| ✅ | Audio                                          | ✅ _Internal Speaker_ and _Headphones_ / _Line in_ <br> ⚠️ _Internal Microphone_ not working <br> Realtek ALC285, layout 11, 21, 31 (all seem to work equal) **TODO supported layouts have changed** ➡️ ``boot-args: alcid=71`` |
| ✅ | iCloud (App Store, iMessage, FaceTime, etc)    | All iServices work |
| ❓ | HiDPI, Handoff, Sidecar                        | Handoff/sidecar sporadic function. Would not rely on these |
| ❌ | Fingerprint Reader                             | Disabled in BIOS to save power |
| ✅ | Power Management Optimizations                 | Fully working with CPUFriend and CPUFriendFriend, more options with YogaSMC to come |
| ✅ | Intel SpeedStep                                | Fully working (Higher performance when plugged in, lower when on battery, tested with GeekBench 5) |

> ✅ Fully functional; ❓ Untested/Intermittent (might work); ❌ Non-functional

</details>


<details>
<summary><strong> LIMITATIONS </strong></summary>
  
Limitations what is not working as expected or improvements:

 - **Bluetooth**: [General Limiations of IntelBluetoothFirmware](https://openintelwireless.github.io/IntelBluetoothFirmware/FAQ.html) + [issue #3](https://github.com/aidanchandra/x1c7-hackintosh/issues/3) 

</details>

<details>
<summary><strong> Common Issues </strong></summary>
  
gioLockscreenState error is a common one working with uncommon iGPUs (like our UHD 620)
See issue #11 for solution

</details>

<br />

## Hardware Compatability
Lenovo allows for many different hardware configurations and will update CPU generations while maintaining the same generation. This makes is somewhat confusing to tell exactly what hardware you have and exactly what this project was meant for. The Shared Hardware tab lists all common hardware info between different models of the X1C7. Following tabs for specific model numbers provide small modificaitons needed to the EFI for their respective model numbers, if needed.

Odds are your hardware will work with minor modifications to the EFI



<details>
<summary><strong> Shared Hardware </strong></summary>

**Again: These are the hardware specs of `20QES01L00` and `20QD-000SUS`:**
Refer to [ThinkPad_X1_Carbon_7th_Gen_Spec.PDF](https://github.com/suhrmann/x1c7-hackintosh/blob/master/docs/references/ThinkPad_X1_Carbon_7th_Gen_Spec.PDF) for possible stock ThinkPad X1 7th Gen configurations. <br>
Source: [Lenovo Product Specification Reference (PSREF) [psref.lenovo.com]](https://psref.lenovo.com/Product/ThinkPad/ThinkPad_X1_Carbon_7th_Gen)


|                  |                 |
| :--------------- | :-------------- |
| **Ports**        | 2x USB 3.1 Gen 1 (Right USB Always On) |
|                  | 2x USB 3.1 Type-C Gen 2 / Thunderbolt 3 (Power Delivery and DisplayPort) [Max 5120x2880 @60Hz] |
|                  | HDMI 1.4b (Max 4096x2160 @24Hz) |                 |
| **Ethernet**     | via ThinkPad Ethernet Extension Adapter Gen 2: I219-LM Ethernet (vPro) |
| **WLAN + BT**    | Intel Wireless-AC 9560, Wi-Fi 2x2 802.11ac + Bluetooth 5.0 |
| **WWAN(optional)** | Nothing else supported, no adapters, nothing. Locked by BIOS |
| **Display**      | 14.0" (355mm) HDR HD (1920 x 1080) |
| **Camera**       | IR and HD720p camera with ThinkShutte. Chicony manufacturer |
| **Audio**        | Realtek ALC3286 codec <br> Linux: ``Realtek ALC285``, layout 11, 21, 31 ; [@acidanthera/AppleALC > Supported codecs [Github]](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs) |
| **Fingerprint reader** | ✔️ |
| **NFC (optional)** | ✔️ |

**Further Specs:**
 - Keyboard: PS/2
 - TrackPoint: PS/2, included alongside te PS2 Keyboard
 - TrackPad: Synaptics enabled i2c
 - **Thunderbolt:**  Intel JHL6540 (Alpine Ridge 4C) Thunderbolt 3 Bridge with what appears to be native MacOS Support

 **NOTE:** The WWAN M.2 slot does **NOT** support SSDs. "If you do manage to fit something in there, you'll be presented with this whitelist error when you try and power the laptop on" [source and photos by @acoutts [Github]](https://github.com/acoutts/x1c7-hackintosh#edit-jan-2-2020) You can modify the bios if you really need the extra SSD.
</details>

<details>
<summary><strong> 20QD-000SUS </strong></summary>
Tested functioning as expected with provided EFI (Aidan's Machine)

| Processor Number                                                                                                                   | Code Name    | # of Cores | # of Threads | Base Frequency | Max Turbo Frequency | Cache | Memory Types | Graphics      |
| :--------------------------------------------------------------------------------------------------------------------------------- | :----------- | :--------- | :----------- | :------------- | :------------------ | :---- | :----------- | :------------ |
| [i7-8665U](https://ark.intel.com/content/www/us/en/ark/products/193563/intel-core-i7-8665u-processor-8m-cache-up-to-4-80-ghz.html) | Whiskey Lake <br>(based on Coffee Lake) | 4          | 8            | 1.9 GHz        | 4.8 GHz             | 8 MB  | LPDDR3-2133  | Intel UHD 620 |

</details>
<details>
<summary><strong> 20QE-S01L00 </strong></summary>
Tested functioning as expected with provided EFI (Simon's Machine)

| Processor Number                                                                                                                   | Code Name    | # of Cores | # of Threads | Base Frequency | Max Turbo Frequency | Cache | Memory Types | Graphics      |
| :--------------------------------------------------------------------------------------------------------------------------------- | :----------- | :--------- | :----------- | :------------- | :------------------ | :---- | :----------- | :------------ |
| [i7-8565U](https://ark.intel.com/content/www/us/en/ark/products/149091/intel-core-i7-8565u-processor-8m-cache-up-to-4-60-ghz.html) | Whiskey Lake <br>(based on Coffee Lake) | 4          | 8            | 1.8 GHz        | 4.6 GHz             | 8 MB  | LPDDR3-2133  | Intel UHD 620 |
</details>

<details>
<summary><strong> 20R1-S05B00 (20R1/20R2) </strong></summary>
Tested functioning as expected with the FOLLOWING MODIFICATIONS
  
```
Cpuid1Data: EC060800 00000000 00000000 00000000
Cpuid1Mask: FFFFFFFF 00000000 00000000 00000000
```

(Credit to @muhchaudhary)

| Processor Number                                                                                                                   | Code Name    | # of Cores | # of Threads | Base Frequency | Max Turbo Frequency | Cache | Memory Types | Graphics      |
| :--------------------------------------------------------------------------------------------------------------------------------- | :----------- | :--------- | :----------- | :------------- | :------------------ | :---- | :----------- | :------------ |
| [i7-10710u](https://ark.intel.com/content/www/us/en/ark/products/196448/intel-core-i7-10710u-processor-12m-cache-up-to-4-70-ghz.html) | Whiskey Lake <br>(based on Coffee Lake) | 6          | 12            | 1.1 GHz        | 4.7 GHz             | 12 MB  | LPDDR3-2133  | Intel® UHD Graphics for 10th Gen Intel® Processors |
</details>
<br /><br />


<!---
# Guide and Resources

There exists a plethora of much more detailed and well maintained guides using Opencore to get MacOS running on a PC. This guide will reference certian parts of those guides frequently for this reason. 


<details>
<summary><strong> Getting Started </strong></summary>

1. [Dortania's Guides](https://dortania.github.io/getting-started/) are the gold standard. We can safely ignore the hardware support function as those are for people who can choose their hardware - we can't with a laptop (yet).
</details> 

<details>
<summary><strong> Create USB Installer </strong></summary>

1. Create a MacOS Catalina Installer on an external USB drive using GibMacOS or using a reagular Mac and the app store. 
2. Mount 

</details> 

<details>
<summary><strong> Getting Started </strong></summary>
</details> 
--->

# Credits
<details>
<summary><strong> OTHER REPOSITORIES </strong></summary>
<br>

- x1c7-hackintosh repositories:
    - [suhrmann/x1c7-hackintosh](https://github.com/suhrmann/x1c7-hackintosh) [fork of tylernguyen/x1c6-hackintosh] _predecessor of this repo_
- x1c6-hackintosh repositories:
    - [tylernguyen/x1c6-hackintosh](https://github.com/tylernguyen/x1c6-hackintosh) 
    - [benbender/x1c6-hackintosh](https://github.com/benbender/x1c6-hackintosh)
    - [zhtengw/EFI-for-X1C6-hackintosh](https://github.com/zhtengw/EFI-for-X1C6-hackintosh)
- t480-hackintosh repositories:
    - [EETagent/T480-OpenCore-Hackintosh](https://github.com/EETagent/T480-OpenCore-Hackintosh)
</details> 

<details>
<summary><strong> CREDITS </strong></summary>
    
**[Acidanthera](https://github.com/acidanthera)** <br> 
For bringing us [OpenCore](https://github.com/acidanthera/OpenCorePkg) and maintaining all the essential kexts, 
like [VirtualSMC](https://github.com/acidanthera/VirtualSMC), [Lilu](https://github.com/acidanthera/Lilu), [WhateverGreen](https://github.com/acidanthera/WhateverGreen), and many many more! 

**Tyler Nguyen [@tylernguyen](https://github.com/tylernguyen)** <br>
 - for his groundwork on [macOS on Thinkpad X1 Carbon 6th Generation, Model 20KH*](
https://github.com/tylernguyen/x1c6-hackintosh)
 - and [lots of documentation](https://github.com/tylernguyen/x1c6-hackintosh/tree/master/docs) about Lenovo and ThinkPads

**[Dortania](https://dortania.github.io/)** <br> 
for his awesome OpenCore guides - here to mention [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) 
and [OpenCore Post-Install](https://dortania.github.io/OpenCore-Post-Install/)
</details>

