# Hackintosh Gigabyte B460M Aorus Pro

README 中文（当前）

[README English](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/blob/master/README_EN.md)

> 2023年3月5日更新：升级 OC 版本至 0.8.9，kext 常规更新，独立显卡更新为 RX6600XT。
>
> 2023年2月7日更新：修复 Ventura 下 Intel 蓝牙问题，取消 debug 信息输出，核显ID变更为 0300C89B（仅用于加速，不用于屏幕输出）。
>
> 2023年1月27日更新：升级 OC 版本至 0.8.8，kext 常规更新。
>
> 2022年11月8日更新：升级 OC 版本至 0.8.5，kext 常规更新，支持 Ventura 正式版。
>
> 2022年10月1日更新：升级 OC 版本至 0.8.4，kext 常规更新。
>
> 2022年5月30日更新：升级 OC 版本至 0.8.0，kext 常规更新。
>
> 2022年3月23日更新：升级 OC 版本至 0.7.9，kext 常规更新。
>
> 2022年1月17日更新：升级 OC 版本至 0.7.7，kext 常规更新。
>
> 2021年12月12日更新：升级 OC 版本至 0.7.6，kext 常规更新。
>
> 2021年11月1日更新：修复 AX200 蓝牙无法正常启闭。
>
> 2021年10月29日更新：修复 ACPI 未加载问题，重新发布。
>
> 2021年10月26日更新：升级 OC 版本至 0.7.4，kext 常规更新，支持 Monterey 正式版。
>
> 2021年9月2日更新：升级 OC 版本至 0.7.2，kext 常规更新。
>
> 2021年7月16日更新：升级 OC 版本至 0.7.1，kext 常规更新。硬件更换更新 CPU 为 10500，增加独立显卡 RX460，修复 UHD630 核显通道号，添加机箱信息。
>
> 2021年4月11日更新：升级 OC 版本至 0.6.8，kext 常规更新。移除独立显卡 RX580，改为核显输出，修复核显 HDMI 接口输出，现支持核显 DP、HDMI 双屏输出。
>
> 2021年3月17日更新：升级 OC 版本至 0.6.7，kext 常规更新。
>
> 2021年1月15日更新：升级 OC 版本至 0.6.5，kext 常规更新。
>
> 2020年11月15日更新：升级 OC 版本至 0.6.3，kext 常规更新，支持在线安装升级至 Big Sur，更新 WIFI kext 为 Big Sur 版。集显 ID 更换为 00009B3E，理论上集显可输出画面（DP 接口）。
>
> 2020年10月15日更新：升级 OC 版本至 0.6.2，kext 常规更新。

## 配置

|   设备   |           型号           |
| :------: | :----------------------: |
|   CPU    |         I5-10500         |
|   主板   | Gigabyte B460M Aorus Pro |
| 集成显卡 |  Intel UHD Graphics 630  |
| 独立显卡 | AMD Radeon RX 6600XT 8G  |
|   声卡   |    Realtek ALCS1200A     |
| 有线网卡 |      Intel I219V12       |
| 无线网卡 |      Wi-Fi 6 AX200       |
|   蓝牙   |    Wi-Fi 6 AX200 自带    |
|   机箱   |     先马趣造 黑色版      |

## 功能

| 功能     | 完成度                  |
| -------- | ----------------------- |
| CPU 变频 | 正常                    |
| 核显     | 硬件加速正常。          |
| 声卡     | 正常，使用 layout-id 50 |
| 网卡     | 正常                    |
| 蓝牙     | 正常                    |
| 睡眠     | 正常                    |
| USB      | 正常                    |

注：USB 端口我屏蔽了后置 USB3 端口的 USB2 支持，加入了机箱前置的 USB3 端口，如有需要请自行定制 USB。

## 已知问题

1. 已知此主板 400 系芯片组，USB 无法直接正常驱动，需要搭配 **XHCI-unsupported.kext** 使用方可正常，本 EFI 已经添加，如有更好方案请告知我，谢谢。

## 主要驱动

|            驱动             |          版本          |
| :-------------------------: | :--------------------: |
|          Lilu.kext          |         1.6.3          |
|       VirtualSMC.kext       |         1.3.0          |
|     WhateverGreen.kext      |         1.6.4          |
|       IntelMausi.kext       |         1.0.7          |
| IntelBluetoothFirmware.kext |         2.2.0          |
|      Airportitlwm.kext      | 2.2.0 Ventura/Monterey |
|        AppleALC.kext        |         1.7.9          |
|           NVMeFix           |         1.1.0          |
|     BlueToolFixup.kext      |         2.6.4          |

注1：AX200 设备相关驱动我已经添加了，**如不需要请自行移除**。

## BIOS 设置

|         关闭         |                  开启                  |
| :------------------: | :------------------------------------: |
|      Fast Boot       |           Above 4G decoding            |
|     Secure Boot      |            Hyper-Threading             |
|   Serial/COM Port    |           EHCI/XHCI Hand-off           |
|         VT-d         |        OS type: Windows 10 WHQL        |
|         CSM          | DVMT Pre-Allocated(iGPU Memory): 128MB |
| Intel Platform Trust |            SATA Mode: AHCI             |
|       CFG Lock       |                                        |
|      Intel SGX       |                                        |

注1：本主板 F3版本 BIOS 中无 CFG Lock 解锁选项，可在安装前在选择界面中选择 **CFG Lock.efi** 进行解锁，后可正常安装。最新版本的 BIOS 中有 CFG 选项，可直接解锁。

注2：注意 Windows10 WHQL 模式可以解决主板 logo 模糊的问题。

## 引导及系统版本

|   项目   |         版本         |
| :------: | :------------------: |
| OpenCore |        0.8.9         |
|  macOS   | Ventura 13.2 (22D49) |

## 安装须知

- 机型我设定为 iMac20,1，有需要请自行更改，另需要自行补充三码。
- **本 EFI 仅适用安装 Big Sur 及以上版本，如有需要请自行修改。**
- DeviceProperties 中核显、声卡、网卡已内建，进系统后请根据需要修正总线地址或移除。
- 理论上来讲本 EFI 在各个品牌 B460M 主板上通用，具体调整请自行解决。
- 使用本 EFI **请务必先阅读上述文字**，完成各项 BIOS 设置，尤其是在 **OC 引导菜单**先解锁 CFG。

## 效果查看

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Desktop.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/codec.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Mic.png)

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Output.png)