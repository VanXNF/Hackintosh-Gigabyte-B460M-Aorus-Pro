# Hackintosh Gigabyte B460M Aorus Pro

[README 中文](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro#readme)

[README English(Current)](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/README_EN.md)

## Hardware

|    Device     |            Model            |
| :-----------: | :-------------------------: |
|      CPU      |          I5-10400           |
|  Motherboard  |  Gigabyte B460M Aorus Pro   |
|     GPU0      |   Intel UHD Graphics 630    |
|     GPU1      | AMD Radeon RX 580 4G 2304sp |
|     Audio     |      Realtek ALCS1200A      |
| Ethernet Card |        Intel I219V12        |
| WiFI/BT Card  |        Wi-Fi 6 AX200        |

## What works

| Function      | Status                       |
| ------------- | ---------------------------- |
| CPU           | Work.                        |
| GPU           | Hardware acceleration works. |
| Audio         | Work with layout-id 50.      |
| Ethernet Card | Work.                        |
| WIFI/BT       | Work.                        |
| Sleep/Wake    | Work.                        |
| USB Mapping   | Work.                        |

## Known issues

1. It is known that the 400 series chipset of this motherboard cannot be directly driven normally. It needs to be used with **XHCI-unsupported.kext** to use USB. This EFI has been added the kext. If there is a better solution, please let me know, thank you.
2. Check that the output of the motherboard's DP interface works. However, I use the UHD630 only for hardware acceleration, so I injected AAPL,ig-platform-id with 0300C89B. **it is suggested to change the id to 00009B3E who only have UHD630. And please debug the HDMI interface yourself**. I have debugged for a long time, but I have not debugged the HDMI interface successfully. At present, the experiment knows that **the index zero is the DP interface, and the Bus-ID is set to 4 to output 4k@60Hz**. If you debug all interfaces successfully, please let me know. Thank you.

## Kexts

|            Kext             |    Model     |
| :-------------------------: | :----------: |
|          Lilu.kext          |    1.4.6     |
|       VirtualSMC.kext       |    1.1.5     |
|     WhateverGreen.kext      |    1.4.1     |
|       IntelMausi.kext       |    1.0.3     |
| IntelBluetoothFirmware.kext |    1.1.2     |
|         itlwmx.kext         | 1.0.0-stable |
|        AppleALC.kext        |    1.5.2*    |

**Note 1:** the AppleALC version is self-compiled 1.5.2, the layout-id is customized for this motherboard, the front and rear microphone and audio output work correctly, the same motherboard is recommended to inject **layout-id 50**, the configuration file has been submitted to the AppleALC's repo, and is expected to be directly supported by the official version 1.5.2. At the same time, it has fixed the problem that the device of the 400 chipset **0xA3F0 driver** cannot be directly driven (see that someone has submitted the PR. 1.5.2 is expected to be supported), **use this version and no need for FakePCIID**.

**Note 2:** I remove the kexts for AX200 by default, **add your own kexts if necessary**.

## BIOS setting

|       Disable        |                Enable                 |
| :------------------: | :-----------------------------------: |
|      Fast Boot       |           Above 4G decoding           |
|     Secure Boot      |            Hyper-Threading            |
|   Serial/COM Port    |          EHCI/XHCI Hand-off           |
|         VT-d         |       OS type: Windows 10 WHQL        |
|         CSM          | DVMT Pre-Allocated(iGPU Memory): 64MB |
| Intel Platform Trust |            SATA Mode: AHCI            |
|       CFG Lock       |                                       |
|      Intel SGX       |                                       |

**Note 1:** The BIOS version is F3, and there is no CFG Lock option in Setting, **So you should select the CFG Lock.efi in OpenCore Picker menu before you try to install the macOS.**

**Note2:** Windows10 WHQL can solve the problem of vague logo of mobo.

## OpenCore/OS

|   Item   |     Version      |
| :------: | :--------------: |
| OpenCore |      0.6.0       |
|  macOS   | Catalina 10.15.6 |

## README Before Install

- I set the model to iMac19,1, please change it if necessary, and you need to add Serial number, UUID and MLB by yourself.
- I have inject GPU0, Audio and Ethernet info in DeviceProperties, modify them if necessary after you login OS.
- In theory, this EFI is universal on each brand B460M motherboard, please solve the specific adjustment on your own.
- Be sure to read the above text **before using this EFI** to complete the BIOS settings, especially to unlock the CFG in the **OC boot menu**.

## Preview

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Desktop.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/macOS.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/codec.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Mic.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Output.png)