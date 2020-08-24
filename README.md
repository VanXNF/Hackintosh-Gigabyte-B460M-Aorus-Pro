# Hackintosh Gigabyte B460M Aorus Pro

[README 中文（当前）](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro#readme)

[README English](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/blob/master/README_EN.md)

## 配置

| 设备 |            型号             |
| :--: | :-------------------------: |
| CPU  |          I5-10400           |
| 主板 |  Gigabyte B460M Aorus Pro   |
| 显卡 |   Intel UHD Graphics 630    |
|      | AMD Radeon RX 580 4G 2304sp |
| 声卡 |      Realtek ALCS1200A      |
| 网卡 |        Intel I219V12        |
|      |        Wi-Fi 6 AX200        |
| 蓝牙 |     Wi-Fi 6 AX200 自带      |

## 功能

| 功能     | 完成度                                    |
| -------- | ----------------------------------------- |
| CPU 变频 | 正常                                      |
| 核显     | 硬件加速正常                              |
| 声卡     | 直接驱动，且使用 id 为 50，完美适配本主板 |
| 网卡     | 正常                                      |
| 蓝牙     | 正常                                      |
| 睡眠     | 正常                                      |
| USB      | 已定制，正常                              |

## 已知问题

1. 已知此主板 400 系芯片组，USB 无法直接正常驱动，需要搭配 **XHCI-unsupported.kext** 使用方可正常，本 EFI 已经添加，如有更好方案请告知我，谢谢。
2. 核显 DP 接口输出正常，当然我日常使用独显 DP 接口，仅将核显用于硬件加速，注入的 AAPL,ig-platform-id 为 0300C89B，**单核显用户建议改为 00009B3E 请自行调试**，我调试了很久也没有调试成功 HDMI 接口，目前实验知**索引第一个为 DP 接口，总线 ID 设为 4 可以 4k@60Hz 输出**，如有成功调试所有接口的希望能告知我，谢谢。

## 主要驱动

|            驱动             |    版本     |
| :-------------------------: | :---------: |
|          Lilu.kext          |    1.4.6    |
|       VirtualSMC.kext       |    1.1.5    |
|     WhateverGreen.kext      |    1.4.1    |
|       IntelMausi.kext       |    1.0.3    |
| IntelBluetoothFirmware.kext |    1.1.2    |
|         itlwmx.kext         | 1.0.0-alpha |
|        AppleALC.kext        |   1.5.2*    |

注1：声卡版本为自编译1.5.2，针对此主板进行了端口定制，前后麦克风和音频输出均正常工作，同主板建议注入 **id 为 50**，配置文件已提交 AppleALC 仓库，预计正式版 1.5.2 直接支持，同时修复了 400 芯片组 **0xA3F0** 设备无法直接驱动的问题（看到有人已经提交了PR，预计 1.5.2 会支持），使用此版本**无需再使用** FakePCIID 进行仿冒，可直接驱动。

注2：AX200 设备相关驱动我**默认移除**了，**如有需要请自行添加**。

## BIOS 设置

|         关闭         |                 开启                  |
| :------------------: | :-----------------------------------: |
|      Fast Boot       |           Above 4G decoding           |
|     Secure Boot      |            Hyper-Threading            |
|   Serial/COM Port    |          EHCI/XHCI Hand-off           |
|         VT-d         |       OS type: Windows 10 WHQL        |
|         CSM          | DVMT Pre-Allocated(iGPU Memory): 64MB |
| Intel Platform Trust |            SATA Mode: AHCI            |
|       CFG Lock       |                                       |
|      Intel SGX       |                                       |

注1：本主板 F3版本 BIOS 中无 CFG Lock 解锁选项，可在安装前在选择界面中选择 **CFG Lock.efi** 进行解锁，后可正常安装。

注2：注意 Windows10 WHQL 模式可以解决主板 logo 模糊的问题。

## 引导及系统版本

|   项目   |       版本       |
| :------: | :--------------: |
| OpenCore |      0.6.0       |
|  macOS   | Catalina 10.15.6 |

## 安装须知

- 机型我设定为 iMac19,1，有需要请自行更改，另需要自行补充三码。
- DeviceProperties 中核显、声卡、网卡已内建，进系统后请根据需要修正总线地址或移除。
- 理论上来讲本 EFI 在各个品牌 B460M 主板上通用，具体调整请自行解决。
- 使用本 EFI **请务必先阅读上述文字**，完成各项 BIOS 设置，尤其是在 **OC 引导菜单**先解锁 CFG。

## 效果查看

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Desktop.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/macOS.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/codec.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Mic.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Output.png)