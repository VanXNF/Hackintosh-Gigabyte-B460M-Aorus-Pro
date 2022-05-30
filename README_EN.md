# Hackintosh Gigabyte B460M Aorus Pro

[README 中文](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro#readme)

README English(Current)

> 2022/5/30 update: OC 0.8.0 and all kexts.
>
> 2022/3/23 update: OC 0.7.9 and all kexts.
>
> 2022/1/17 update: OC 0.7.7 and all kexts.
>
> 2021/12/12 update: OC 0.7.6 and all kexts.
>
> 2021/11/1 update: fix AX200 bluetooth.
>
> 2021/10/29 update: fix ACPI not enable and release.
>
> 2021/10/26 update: OC 0.7.4 and all kexts. Support Monterey now.
>
> 2021/9/2 update: OC 0.7.2 and all kexts.
>
> 2021/7/16 update: OC 0.7.1 and all kexts. Update I5-10500, add RX460 4G, fix UHD 630 FB, add case information.
>
> 2021/4/11 update: OC 0.6.8 and all kexts. AMD RX580 has been removed, UHD630 is used to drive a display (support DP and HDMI).
>
> 2021/3/17 update: OC 0.6.7 and all kexts.
>
> 2021/1/15 update: OC 0.6.5 and all kexts.
>
> 2020/11/15 update: OC 0.6.3 and all kexts, support update to Big Sur in current system. now. change WIFI kext to Big Sur version, and AAPL,ig-platform-id now set 00009B3E as default, so you can use UHD630 to drive screens (DP).
>
> 2020/10/15 update: OC 0.6.2 and all kexts.

## Hardware

|    Device     |          Model           |
| :-----------: | :----------------------: |
|      CPU      |         I5-10500         |
|  Motherboard  | Gigabyte B460M Aorus Pro |
|     GPU0      |  Intel UHD Graphics 630  |
|     GPU1      |   AMD Radeon RX 460 4G   |
|     Audio     |    Realtek ALCS1200A     |
| Ethernet Card |      Intel I219V12       |
| WiFI/BT Card  |      Wi-Fi 6 AX200       |
|     Case      |     先马趣造 黑色版      |

## What works

| Function      | Status                                     |
| ------------- | ------------------------------------------ |
| CPU           | Work.                                      |
| GPU           | DP, HDMI work. Hardware acceleration work. |
| Audio         | Work with layout-id 50.                    |
| Ethernet Card | Work.                                      |
| WIFI/BT       | Work.                                      |
| Sleep/Wake    | Work.                                      |
| USB Mapping   | Work.                                      |

**Note 1:** To meet the limit of 15 usb ports on macOS, I had to limit the usb2 of the usb3 Port on the back of the chassis, I have 2 usb3 ports on the front panel that they can respond to both usb2 and usb3 devices. To sum up, usb3 ports on the back of the chassis can only respond to usb3 devices. So that **you should connect keyboard and mouse with usb2 ports.** You should remap your own USB ports if my mapping can not work properly.

## Known issues

1. It is known that the 400 series chipset of this motherboard cannot be directly driven normally. It needs to be used with **XHCI-unsupported.kext** to use USB. This EFI has been added the kext. If there is a better solution, please let me know, thank you.

## Kexts

|            Kext             |          Version           |
| :-------------------------: | :------------------------: |
|          Lilu.kext          |           1.6.0            |
|       VirtualSMC.kext       |           1.2.9            |
|     WhateverGreen.kext      |           1.5.8            |
|       IntelMausi.kext       |           1.0.7            |
| IntelBluetoothFirmware.kext |           2.1.0            |
|      Airportitlwm.kext      | 2.1.0 Monterey and Big Sur |
|        AppleALC.kext        |           1.7.1            |
|           NVMeFix           |           1.0.9            |
|     BlueToolFixup.kext      |           2.6.1            |

**Note 1:** I add the kexts for AX200 by default, **remove your own kexts if necessary**.

## BIOS setting

|       Disable        |                 Enable                 |
| :------------------: | :------------------------------------: |
|      Fast Boot       |           Above 4G decoding            |
|     Secure Boot      |            Hyper-Threading             |
|   Serial/COM Port    |           EHCI/XHCI Hand-off           |
|         VT-d         |        OS type: Windows 10 WHQL        |
|         CSM          | DVMT Pre-Allocated(iGPU Memory): 128MB |
| Intel Platform Trust |            SATA Mode: AHCI             |
|       CFG Lock       |                                        |
|      Intel SGX       |                                        |

**Note 1:** The BIOS version is F3, and there is no CFG Lock option in Setting, **So you should select the CFG Lock.efi in OpenCore Picker menu before you try to install the macOS.**

**Note2:** Windows10 WHQL can solve the problem of vague logo of mobo.

## OpenCore/OS

|   Item   |        Version        |
| :------: | :-------------------: |
| OpenCore |         0.8.0         |
|  macOS   | Monterey 12.4 (21F79) |

## README Before Install

- I set the model to iMac20,1, please change it if necessary, and you need to add Serial number, UUID and MLB by yourself.
- I have inject GPU0, Audio and Ethernet info in DeviceProperties, modify them if necessary after you login OS.
- In theory, this EFI is universal on each brand B460M motherboard, please solve the specific adjustment on your own.
- Be sure to read the above text **before using this EFI** to complete the BIOS settings, especially to unlock the CFG in the **OC boot menu**.

## Preview

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Desktop.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/codec.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Mic.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Output.png)
