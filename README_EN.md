# Hackintosh Gigabyte B460M Aorus Pro

[README 中文](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro#readme)

README English(Current)

> 2023/4/4 update: OC 0.9.1 and all kexts. Update README.

## README Before Install

- Theoretically, this EFI should be compatible with B460M motherboards from different brands. However, please make the necessary adjustments on your own.

- Before using this EFI, please read the following instructions thoroughly and adjust the BIOS settings accordingly.

### BIOS Setting

|         关闭         |                  开启                  |
| :------------------: | :------------------------------------: |
|      Fast Boot       |           Above 4G decoding            |
|     Secure Boot      |            Hyper-Threading             |
|   Serial/COM Port    |           EHCI/XHCI Hand-off           |
|         CSM          |        OS type: Windows 10 WHQL        |
| Intel Platform Trust | DVMT Pre-Allocated(iGPU Memory): 128MB |
|       CFG Lock       |            SATA Mode: AHCI             |
|      Intel SGX       |          Re-Size BAR Support           |
|                      |                  VT-d                  |

> Note 1: The latest version of BIOS includes a CFG option that can be unlocked directly.
> Note 2: Please note that Windows 10 WHQL mode can address the issue of blurry motherboard logo.
> Note 3: Re-Size BAR Support and VT-d can be disabled, for further information please refer to `EFI modification instructions`.

### EFI Modification Instructions

#### Booter Part

In the Quirks option, after enabling Resizable BAR Support in the BIOS, this EFI will change ResizeAppleGpuBars from `-1` to `0`. For those who have not enabled or cannot enable it in the BIOS, please set this option to `-1`.

#### DeviceProperties Part

In this EFI, the integrated graphics card, sound card, and network card have been built-in through DeviceProperties. Users who use this EFI can modify the bus address or remove them according to their needs after entering the system.

- The content in `PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)` refers to the RX6600XT graphics card buffer frame settings. It specifies a buffer frame to lock the memory frequency to prevent coil whine. Users who do not experience coil whine with their graphics cards or don't use this particular model of graphics card can remove this entry.

- The content in `PciRoot(0x0)/Pci(0x1C,0x0)/Pci(0x0,0x0)` refers to the Wi-Fi card AX200, which is used for built-in devices.
- The content in `PciRoot(0x0)/Pci(0x1F,0x3)` refers to the sound card Realtek ALCS1200A, with the layout-id set to 50.
- The content in `PciRoot(0x0)/Pci(0x1F,0x6)` refers to the Ethernet Connection (12) I219-V wired network card, which is used for built-in devices.
- The content in `PciRoot(0x0)/Pci(0x2,0x0)` refers to the UHD 630 graphics card buffer frame settings. In this case, a buffer frame without video output is used, with the code `0300C89B`. Users who require video output should make the necessary modifications.

 #### Kernel Part

If VT-D is enabled in the BIOS, this EFI will enable `DisableIoMapper` in the Quirks option. If the user disables the VT-D option in the BIOS, the value can be set to `false` here.

> In addition, this EFI employs the AirportItlwm related kexts, which demand a certain loading sequence. The existing loading order has been tested to be relatively stable, so it is advised not to adjust the sequence unnecessarily.

#### Misc Part

After multiple iterations, this EFI has become more stable, and therefore, the debugging information output has been disabled. For those who need debugging during the initial installation, the `AppleDebug` and `ApplePanic` options can be enabled in the `Debug` section. The `Target` value should be specified as `67` for log output.

> Under `Tools`, there is a CFG unlock tool called `CFGLock.efi`, which can be used for CFG unlocks on earlier BIOS versions.

#### NVRAM Part

Users with a resolution lower than 4K can adjust the `UIScale` value in `4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14` to `01`.

Users who do not use RX5000/RX6000 series graphics cards need to remove the `agdpmod=pikera` parameter in `7C436110-AB2A-4BBB-A880-FE41995C9F82`'s `boot-args`.

Users who need to enable DEBUG should add the parameter `-v debug=0x100 keepsyms=1` in `7C436110-AB2A-4BBB-A880-FE41995C9F82`'s `boot-args`.

#### PlatformInfo Part

This EFI sets the model to **iMac20,1**, and if necessary, users can change it themselves.

Users who need to log in to an Apple account **must change** the `SystemSerialNumber`, `SystemUUID`, and `MLB` themselves.

### Power-related settings

To effectively prevent system crashes caused by sleep, the following commands can be executed in the terminal or the HackinTool can be used to repair the power options.

```shell
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
```

## Hardware

|    Device     |          Model           |
| :-----------: | :----------------------: |
|      CPU      |         I5-10500         |
|  Motherboard  | Gigabyte B460M Aorus Pro |
|      RAM      | JUHOR DDR4 2666MHz 16G X4|
|     GPU0      |  Intel UHD Graphics 630  |
|     GPU1      | AMD Radeon RX 6600XT 8G  |
|     Audio     |    Realtek ALCS1200A     |
| Ethernet Card |      Intel I219V12       |
| WiFI/BT Card  |      Wi-Fi 6 AX200       |
|     Case      |     SAMA 趣造 I'm         |

## What works

| Function      | Status                      |
| ------------- | --------------------------- |
| CPU           | Work.                       |
| GPU           | Hardware acceleration work. |
| Audio         | Work with layout-id 50.     |
| Ethernet Card | Work.                       |
| WIFI/BT       | Work.                       |
| Sleep/Wake    | Work.                       |
| USB Mapping   | Work.                       |

**Note 1:** To meet the limit of 15 usb ports on macOS, I had to limit the usb2 of the usb3 Port on the back of the chassis, I have 2 usb3 ports on the front panel that they can respond to both usb2 and usb3 devices. To sum up, usb3 ports on the back of the chassis can only respond to usb3 devices. So that **you should connect keyboard and mouse with usb2 ports.** You should remap your own USB ports if my mapping can not work properly.

## Kexts

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/kernels.png)

## OpenCore/OS

|   Item   |        Version        |
| :------: | :-------------------: |
| OpenCore |         0.9.1         |
|  macOS   | Ventura 13.3 (22E252) |

## Preview

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/desktop.png)

### Update Log

- 2023/4/4 update: OC 0.9.1 and all kexts. Update README.
- 2023/3/5 update: OC 0.8.9 and all kexts. Remove RX 460, add RX 6600XT.

- 2023/2/7 update: Fix Intel Bluetooth issue(Ventura), diable debug output, UHD630 use ID 0300C89B(UHD630 used for acceleration only).

- 2023/1/27 update: OC 0.8.8 and all kexts.

- 2022/11/8 update: OC 0.8.5 and all kexts. Support Ventura Now.

- 2022/10/1 update: OC 0.8.4 and all kexts.

- 2022/5/30 update: OC 0.8.0 and all kexts.

- 2022/3/23 update: OC 0.7.9 and all kexts.

- 2022/1/17 update: OC 0.7.7 and all kexts.

- 2021/12/12 update: OC 0.7.6 and all kexts.

- 2021/11/1 update: fix AX200 bluetooth.

- 2021/10/29 update: fix ACPI not enable and release.

- 2021/10/26 update: OC 0.7.4 and all kexts. Support Monterey now.

- 2021/9/2 update: OC 0.7.2 and all kexts.

- 2021/7/16 update: OC 0.7.1 and all kexts. Update I5-10500, add RX460 4G, fix UHD 630 FB, add case information.

- 2021/4/11 update: OC 0.6.8 and all kexts. AMD RX580 has been removed, UHD630 is used to drive a display (support DP and HDMI).

- 2021/3/17 update: OC 0.6.7 and all kexts.

- 2021/1/15 update: OC 0.6.5 and all kexts.

- 2020/11/15 update: OC 0.6.3 and all kexts, support update to Big Sur in current system. now. change WIFI kext to Big Sur version, and AAPL,ig-platform-id now set 00009B3E as default, so you can use UHD630 to drive screens (DP).

- 2020/10/15 update: OC 0.6.2 and all kexts.
