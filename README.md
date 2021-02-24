# macOS on Thinkpad X1 Carbon 7th Generation, Model 20QE*

OpenCore-based Hackintosh EFI and guide for Lenovo Thinkpad X1 Carbon Gen 7


# What's working

In short, x1c7-hackintosh is very stable and is currently my daily driver. I fully recommend this project to anyone looking for a MacBook alternative.

### Install
| working | Device / Step                             | Comment            |
|:-------:|:------------------------------------------|:-------------------|
| ☑️ | **Basic Setup**                                 |                    |
| ✅ | Booting macOS installer                        |                    |
| ✅ | Installed to HD                                |                    |

### Post-Install

| working | Device / Step                             | Comment            |
|:-------:|:------------------------------------------|:-------------------|
| ✅ | Graphics                                       | Requires `WhateverGreen.kext` |
| ✅ | Touchpad                                       | Requires ``VoodooI2C`` (?) |
| ✅ | Trackpoint                                     | Requires ``VoodooPS2`` (?) |
| ✅ | Keyboard                                       | Requires ``VoodooPS2`` (?) |
| ✅ | Keyboard-Multimedia Fn keys                    | Requires `YogaSMC.kext` + **TODO: Add ACPI here** and [YogaSMC-App](https://github.com/zhen-zen/YogaSMC) |
| ✅ | WiFi                                           | Native WiFi with `AirportItlwm.kext` - no companion app required |
| ❌ | Bluetooth                                      | `IntelBluetoothFirmware` might depend on `AirportItlwm`, that requires Apple's secure boot  |
| ❌ | WWAN                                           | DISABLED at BIOS |
| ✅ | Ethernet                                       | `IntelMausi.kext` for bundled USB-C adapter |
| ✅ | Hibernation                                    |           |
| ✅ | HDMI output                                    | Requires **WEG?** |
| ✅ | USB A / USB C                                  |           |
| ✅ | Thunderbolt 3                                  |           |
| ❌ | Webcam                                         | _checked on 2021-02-19_ |
| ✅ | Audio                                          | ✅ _Internal Speaker_ and _Headphones_ / _Line in_ <br> ⚠️ _Internal Microphone_ not working <br> Realtek ALC285, layout 11, 21, 31 (all seem to work equal) **TODO supported layouts have changed** ➡️ ``boot-args: alcid=21`` |
| ❓ | iCloud (App Store, iMessage, FaceTime, etc)    |           |
| ❓ | HiDPI, Handoff, Sidecar                        |           |
| ❌ | Fingerprint Reader                             |           |
| ❓ | Power Management Optimizations                 |           |

> ✅ Fully functional; ❓ Untested (might work); ❌ Non-functional


# Credits

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


