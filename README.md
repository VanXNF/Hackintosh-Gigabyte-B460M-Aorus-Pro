# Hackintosh Gigabyte B460M Aorus Pro

README 中文（当前）

[README English](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/blob/master/README_EN.md)

> - 2023年4月4日更新：升级 OC 版本至 0.9.1，kext 常规更新，更新 README 说明。

## 使用说明

- 理论上来讲本 EFI 在各个品牌 B460M 主板上通用，具体调整请自行解决。
- 使用本 EFI **请务必先阅读下列说明文字**，完成各项 BIOS 设置。

### BIOS 设置

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

> 注1：最新版本的 BIOS 中有 CFG 选项，可直接解锁。
>
> 注2：注意 Windows10 WHQL 模式可以解决主板 logo 模糊的问题。
>
> 注3：Re-Size BAR Support 和 VT-d 可选关闭，具体参见 `EFI相关修改说明`。

### EFI 相关修改说明

#### Booter 部分

在 Quirks 选项中，在 BIOS 中启用 Resizable BAR Support 后，本 EFI 将 ResizeAppleGpuBars 从 `-1`更改为`0`，未开启或 BIOS 中无法开启的请将此项设置为 `-1`。

#### DeviceProperties 部分

本 EFI 在 DeviceProperties 中对核显、声卡、网卡进行了内建，使用本 EFI 的用户可在进系统后根据需要修正总线地址或移除。

- `PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)`中的内容对应的是独立显卡部分 RX6600XT 缓冲帧相关设置，通过指定缓冲帧锁定显存频率防止电感啸叫，显卡无啸叫情况的或未使用此型号显卡的用户可移除此条目。

- `PciRoot(0x0)/Pci(0x1C,0x0)/Pci(0x0,0x0)`中的内容对应的是 Wi-Fi 网卡 AX200，用于内建设备。
- `PciRoot(0x0)/Pci(0x1F,0x3)`中的内容对应的是声卡 Realtek ALCS1200A，此处指定了使用 layout-id 50。
- `PciRoot(0x0)/Pci(0x1F,0x6)`中的内容对应的是有线网卡 Ethernet Connection (12) I219-V，用于内建设备。
- `PciRoot(0x0)/Pci(0x2,0x0)`中的内容对应的是核显 UHD 630 缓冲帧相关设置，此处使用了无显示输出缓冲帧 `0300C89B`，有显示输出需要的请自行修改。

 #### Kernel 部分

在 BIOS 中开启 VT-D 后，本 EFI 在 Quirks 选项中启用了 `DisableIoMapper`，用户如在 BIOS 中禁用了 VT-D 选项，可在此处将值设置为`false`。

> 此外，本 EFI 使用了 AirportItlwm 相关驱动，对于驱动加载顺序有一定要求，现有加载顺序经过测试较为稳定，如无必要请不要调整顺序。

#### Misc 部分

经过多次迭代，本 EFI 已较为稳定，因此取消了 DEBUG 调试信息输出，初次安装或有调试需要可以在 `Debug` 部分，开启 `AppleDebug`、`ApplePanic` 选项，并指定日志输出 `Target` 值为 `67`。

> `Tools` 下有 CFG 解锁工具 `CFGLock.efi`，可用于早期 BIOS 版本的 CFG 解锁。

#### NVRAM 部分

使用 4K 以下分辨率屏幕的用户可以将  `4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14` 中的 `UIScale` 值调整为 `01`。

未使用 RX5000/RX6000 系列显卡的用户需要将 `7C436110-AB2A-4BBB-A880-FE41995C9F82` 中的 `boot-args` 中的 `agdpmod=pikera` 参数移除。

需要启用 DEBUG 的用户需要在 `7C436110-AB2A-4BBB-A880-FE41995C9F82` 中的 `boot-args` 中添加参数 `-v debug=0x100 keepsyms=1`。

#### PlatformInfo 部分

本 EFI 将机型设定为 **iMac20,1**，有需要请自行更改。

需要登录Apple账户的用户**必须自行更换** `SystemSerialNumber`、`SystemUUID` 和 `MLB`。

### 电源相关设置

在终端依次执行以下指令或使用 HackinTool 修复电源选项，可有效防止睡眠引发系统崩溃。

```shell
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
```

## 配置信息

|      设备       |           型号            |
| :-------------: | :-----------------------: |
|       CPU       |         I5-10500          |
|      主板       | Gigabyte B460M Aorus Pro  |
|      内存       | JUHOR DDR4 2666MHz 16G X4 |
|    集成显卡     |  Intel UHD Graphics 630   |
|    独立显卡     |  AMD Radeon RX 6600XT 8G  |
|      声卡       |     Realtek ALCS1200A     |
|    有线网卡     |       Intel I219V12       |
| 无线网卡 & 蓝牙 |       Wi-Fi 6 AX200       |
|      机箱       |       SAMA 趣造 I'm       |


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

## 主要驱动及加载顺序

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Kernels.png)

## 引导及系统版本

|   项目   |         版本          |
| :------: | :-------------------: |
| OpenCore |         0.9.1         |
|  macOS   | Ventura 13.3 (22E252) |

## 效果查看

![](https://github.com/VanXNF/Hackintosh-Gigabyte-B460M-Aorus-Pro/raw/master/Images/Desktop.png)

## 更新日志

- 2023年4月4日更新：升级 OC 版本至 0.9.1，kext 常规更新，更新 README 说明。

- 2023年3月5日更新：升级 OC 版本至 0.8.9，kext 常规更新，独立显卡更新为 RX6600XT。

- 2023年2月7日更新：修复 Ventura 下 Intel 蓝牙问题，取消 debug 信息输出，核显ID变更为 0300C89B（仅用于加速，不用于屏幕输出）。

- 2023年1月27日更新：升级 OC 版本至 0.8.8，kext 常规更新。

- 2022年11月8日更新：升级 OC 版本至 0.8.5，kext 常规更新，支持 Ventura 正式版。

- 2022年10月1日更新：升级 OC 版本至 0.8.4，kext 常规更新。

- 2022年5月30日更新：升级 OC 版本至 0.8.0，kext 常规更新。

- 2022年3月23日更新：升级 OC 版本至 0.7.9，kext 常规更新。

- 2022年1月17日更新：升级 OC 版本至 0.7.7，kext 常规更新。

- 2021年12月12日更新：升级 OC 版本至 0.7.6，kext 常规更新。

- 2021年11月1日更新：修复 AX200 蓝牙无法正常启闭。

- 2021年10月29日更新：修复 ACPI 未加载问题，重新发布。

- 2021年10月26日更新：升级 OC 版本至 0.7.4，kext 常规更新，支持 Monterey 正式版。

- 2021年9月2日更新：升级 OC 版本至 0.7.2，kext 常规更新。

- 2021年7月16日更新：升级 OC 版本至 0.7.1，kext 常规更新。硬件更换更新 CPU 为 10500，增加独立显卡 RX460，修复 UHD630 核显通道号，添加机箱信息。

- 2021年4月11日更新：升级 OC 版本至 0.6.8，kext 常规更新。移除独立显卡 RX580，改为核显输出，修复核显 HDMI 接口输出，现支持核显 DP、HDMI 双屏输出。

- 2021年3月17日更新：升级 OC 版本至 0.6.7，kext 常规更新。

- 2021年1月15日更新：升级 OC 版本至 0.6.5，kext 常规更新。

- 2020年11月15日更新：升级 OC 版本至 0.6.3，kext 常规更新，支持在线安装升级至 Big Sur，更新 WIFI kext 为 Big Sur 版。集显 ID 更换为 00009B3E，理论上集显可输出画面（DP 接口）。

- 2020年10月15日更新：升级 OC 版本至 0.6.2，kext 常规更新。
