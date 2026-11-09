# HUAWEI MateBook 14 2021 (KLVD-WXX9) · macOS Sonoma OpenCore EFI

华为 MateBook 14 2021 黑苹果 EFI / Hackintosh EFI for HUAWEI MateBook 14 2021

**中文 / English**（本文档中英双语 / This document is bilingual）

## 简介 / Overview

**中文**：本仓库是本机当前实际使用的 OpenCore EFI 备份，系统为 macOS Sonoma 14.8.9（23J631）。EFI 已在本机验证可启动；显卡加速、Wi-Fi、蓝牙、触控板、键盘、电池、亮度、摄像头、USB、NVMe 等可用，音频目前尚未驱动。

**English**: This repository contains the OpenCore EFI currently used on my HUAWEI MateBook 14 2021 (KLVD-WXX9), running macOS Sonoma 14.8.9 (23J631). It boots on this exact machine. Graphics acceleration, Wi-Fi, Bluetooth, trackpad, keyboard, battery, brightness, camera, USB and NVMe are working; audio is not working yet.

> **注意 / Note**：仓库中的 `config.plist` 已移除原机 SMBIOS 信息（序列号、MLB、UUID、ROM），替换为占位值。使用前请自行生成三码。 / SMBIOS identifiers in `config.plist` have been sanitized (serial, MLB, UUID and ROM replaced with placeholders). Generate your own SMBIOS before use.

## 硬件 / Hardware

| 项目 / Item | 规格 / Specification |
| --- | --- |
| 机型 / Model | HUAWEI MateBook 14 2021 (KLVD-WXX9 / C233) |
| CPU | Intel Core i5-1135G7, 4C/8T, 2.4 GHz (Tiger Lake) |
| 核显 / iGPU | Intel Iris Xe Graphics (Device ID `0x9A49`) |
| 内存 / RAM | 16 GB |
| 硬盘 / Storage | 512 GB NVMe SSD (PCIe 8 GT/s x4) |
| 屏幕 / Display | 14-inch 2160 × 1440 (3:2), internal |
| Wi-Fi | Intel Wi-Fi 6 AX201 (`8086:A0F0`) |
| 蓝牙 / Bluetooth | Intel AX201 Bluetooth (`8087:0026`) |
| 摄像头 / Camera | Quanta HD Camera (`0408:1042`) |
| 指纹 / Fingerprint | Goodix (`27C6:5125`), unsupported |
| 音频 / Audio | Intel Tiger Lake HDA/SST, currently not working |

## 软件与引导 / Software & Boot

| 项目 / Item | 值 / Value |
| --- | --- |
| macOS | 14.8.9 Sonoma (23J631) |
| OpenCore | 1.0.8 (`REL-108-2026-09-27`) |
| SMBIOS | `MacBookPro16,2` (`Mac-5F9802EFE386AA28`) |
| SIP | `0x803` (partial / 部分关闭) |
| SecureBootModel | Disabled |
| Vault | Optional |
| 启动参数 / boot-args | `-v keepsyms=1 debug=0x100 -allow3d -disablegfxfirmware -vi2c-force-polling applbkl=1 -igfxbls -igfxdvmt -wex-edp-fix-training` |

> 当前保留了调试用启动参数（`-v keepsyms=1 debug=0x100`）。日常使用可在 `NVRAM → Add → 7C436110-AB2A-4BBB-A880-FE41995C9F82 → boot-args` 中删除这两项。 / Debug boot arguments (`-v keepsyms=1 debug=0x100`) are intentionally left enabled. Remove them from `NVRAM → Add → 7C436110-AB2A-4BBB-A880-FE41995C9F82 → boot-args` for daily use.

## 驱动状态 / Driver Status

| 功能 / Feature | 状态 / Status | 说明 / Notes |
| --- | --- | --- |
| 核显加速 / Graphics | ✅ 正常 / Working | Intel Iris Xe, Metal 2, 1536 MB VRAM, 2160 × 1440 |
| Wi-Fi | ✅ 正常 / Working | AirportItlwm 2.3.0; connected. AirDrop/Handoff not supported |
| 蓝牙 / Bluetooth | ✅ 正常 / Working | IntelBluetoothFirmware 2.4.0; devices detected |
| 音频 / Audio | ❌ 不正常 / Not working | AppleALC 1.9.8 + `layout-id=70`, but AppleHDA is not loaded and there are no output devices |
| 触控板 / Trackpad | ✅ 正常 / Working | VoodooI2C 2.9.1 + VoodooI2CHID (I2C HID) |
| 键盘 / Keyboard | ✅ 正常 / Working | VoodooPS2Controller 2.3.7 |
| 电池 / Battery | ✅ 正常 / Working | SMCBatteryManager 1.3.8; charge status works |
| 亮度 / Brightness | ✅ 正常 / Working | SSDT-PNLF + BrightnessKeys; AppleBacklightDisplay registered |
| 摄像头 / Camera | ✅ 正常 / Working | Quanta HD Camera |
| USB | ✅ 正常 / Working | USBToolBox + UTBDefault mapping |
| NVMe | ✅ 正常 / Working | NVMeFix 1.1.3 |
| CPU 电源管理 / CPU PM | ✅ 正常 / Working | SSDT-PLUG; X86PlatformPlugin loaded |
| 睡眠唤醒 / Sleep & Wake | ⚠️ 未测试 / Untested | Please test on your machine |
| HDMI / 外接显示 / External display | ⚠️ 未测试 / Untested | `tgl-hdmi-wiring` / `tgl-tc-wiring` are set |
| 指纹 / Fingerprint | ❌ 不支持 / Unsupported | Goodix `27C6:5125`, no macOS driver |
| AirDrop / Handoff | ❌ 不支持 / Unsupported | Limitation of Intel Wi-Fi / AirportItlwm |
| iServices | ⚠️ 需自行配置 / Configure yourself | Generate your own SMBIOS first |

## EFI 内容 / What's Inside

```text
EFI/
├── BOOT/
│   └── BOOTx64.efi
└── OC/
    ├── ACPI/           9 SSDTs
    ├── Drivers/        OpenRuntime.efi, HfsPlus.efi
    ├── Kexts/          22 kexts
    ├── Tools/          OpenShell.efi
    ├── OpenCore.efi
    └── config.plist
```

> 仓库只包含 OpenCore 相关部分（`EFI/BOOT` + `EFI/OC`）。原 EFI 分区中的 `EFI/Microsoft` 是 Windows 引导文件，未包含在本仓库中。 / Only the OpenCore parts (`EFI/BOOT` + `EFI/OC`) are included. The `EFI/Microsoft` folder on the original ESP is the Windows bootloader and is not part of this repository.

### ACPI

- `SSDT-ALS0.aml` — ambient light sensor
- `SSDT-EC.aml` — embedded controller
- `SSDT-I2C0PS3.aml` — I2C0 touchpad
- `SSDT-PLUG.aml` — CPU power management
- `SSDT-PNLF.aml` — backlight
- `SSDT-RTCAWAC.aml` — RTC/AWAC
- `SSDT-SBUS-MCHC.aml` — SMBus
- `SSDT-USBX.aml` — USB power properties
- `SSDT-XOSI.aml` — OSI compatibility

### Drivers

- `OpenRuntime.efi` — required by OpenCore
- `HfsPlus.efi` — HFS+ filesystem driver
- `ResetNvramEntry.efi` — disabled by default
- `ToggleSipEntry.efi` — disabled by default

### Kexts

| Kext | 版本 / Version | 用途 / Purpose |
| --- | --- | --- |
| Lilu | 1.7.2 | Kernel patching framework |
| VirtualSMC | 1.3.8 | SMC emulation |
| SMCProcessor | 1.3.8 | CPU sensors |
| SMCSuperIO | 1.3.8 | Super I/O sensors |
| SMCBatteryManager | 1.3.8 | Battery status |
| SMCLightSensor | 1.3.8 | Ambient light sensor |
| WhateverXe | 0.8.0 | Intel Xe graphics (includes AppleIntelTGLCompat) |
| AMFIPass | 1.4.1 | AMFI/SIP compatibility |
| AirportItlwm | 2.3.0 | Intel Wi-Fi |
| IntelBluetoothFirmware | 2.4.0 | Intel Bluetooth firmware |
| IntelBTPatcher | 2.4.0 | Intel Bluetooth patches |
| BlueToolFixup | 2.7.2 | Bluetooth fixes |
| VoodooPS2Controller | 2.3.7 | PS/2 keyboard and trackpad |
| VoodooI2C | 2.9.1 | I2C controller support |
| VoodooI2CHID | 1.0 | I2C HID touchpad |
| AppleALC | 1.9.8 | Audio (currently not working) |
| USBToolBox | — | USB mapping |
| UTBDefault | 1.0 | Default USB map |
| ECEnabler | 1.0.6 | EC battery reading |
| NVMeFix | 1.1.3 | NVMe power management |
| CpuTscSync | 1.1.2 | TSC synchronization |
| BrightnessKeys | — | Brightness keys |

## 使用说明 / Usage

1. **备份 / Backup**：先备份你现有的 EFI 分区。 / Back up your existing EFI partition first.
2. **挂载 EFI / Mount the ESP**：`sudo diskutil mount disk0s1`（或 `sudo diskutil mount EFI`）。 / `sudo diskutil mount disk0s1` (or `sudo diskutil mount EFI`).
3. **复制 / Copy**：把本仓库的 `EFI/BOOT` 和 `EFI/OC` 复制到 EFI 分区根目录。 / Copy `EFI/BOOT` and `EFI/OC` from this repository to the root of your ESP.
4. **生成三码 / Generate SMBIOS**：使用 GenSMBIOS 或 OCAT 生成 `MacBookPro16,2` 的三码，替换 `config.plist → PlatformInfo → Generic` 中的 `SystemSerialNumber`、`MLB`、`SystemUUID`、`ROM`。仓库中的值是占位值。 / Use GenSMBIOS or OCAT to generate a `MacBookPro16,2` SMBIOS, then replace `SystemSerialNumber`, `MLB`, `SystemUUID` and `ROM` under `config.plist → PlatformInfo → Generic`. The values in this repository are placeholders.
5. **BIOS 设置 / BIOS settings**：关闭 Secure Boot 和 Fast Boot；如果 BIOS 允许，建议关闭 VT-d 和 CFG Lock。本 EFI 已启用 `DisableIoMapper` 和 `AppleXcpmCfgLock`。 / Disable Secure Boot and Fast Boot. If your BIOS allows it, disable VT-d and CFG Lock as well. This EFI already enables `DisableIoMapper` and `AppleXcpmCfgLock`.
6. **日常使用 / Daily use**：如不需要调试，删除 `-v keepsyms=1 debug=0x100` 启动参数。 / Remove `-v keepsyms=1 debug=0x100` from `boot-args` if you do not need verbose/debug output.
7. **重置 NVRAM / Reset NVRAM**：`ResetNvramEntry.efi` 默认关闭；如需要，可在 `config.plist → UEFI → Drivers` 中临时启用。 / `ResetNvramEntry.efi` is disabled by default. Enable it temporarily under `config.plist → UEFI → Drivers` if needed.

> 本 EFI 是针对该机型定制的。不要直接用于其他笔记本；ACPI、USB 映射和 SMBIOS 需要按机型调整。 / This EFI is tailored to this exact laptop. Do not use it on other models without adjusting ACPI, USB mapping and SMBIOS.

## 已知问题 / Known Issues

- **音频 / Audio**：目前没有音频输出设备。AppleALC 已加载并注入 `layout-id=70`，但 AppleHDA 未加载，问题仍在排查。 / No audio output devices at the moment. AppleALC is loaded with `layout-id=70`, but AppleHDA is not loaded. Still investigating.
- **指纹 / Fingerprint**：Goodix 指纹识别无 macOS 驱动，不支持。 / The Goodix fingerprint reader has no macOS driver and is unsupported.
- **AirDrop / Handoff / 接力**：Intel Wi-Fi 方案（AirportItlwm）不支持这些隔空功能。 / AirDrop, Handoff and Continuity are not supported by the Intel Wi-Fi solution (AirportItlwm).
- **睡眠唤醒 / Sleep & Wake**：未测试，欢迎反馈。 / Not tested yet; feedback is welcome.
- **HDMI / 外接显示 / External display**：未测试。 / Not tested yet.
- **iServices**：仓库已移除原机 SMBIOS，必须自行生成三码后才能登录 iCloud/iMessage。 / The original SMBIOS has been removed; generate your own identifiers before signing in to iCloud/iMessage.
- **调试参数 / Debug boot-args**：默认保留 verbose/debug，日常使用建议删除。 / Verbose/debug boot arguments are enabled by default; remove them for daily use.

## 免责声明 / Disclaimer

**中文**：本项目仅供学习、研究和交流使用，与 Apple Inc. 和 HUAWEI 无任何关联。macOS 是 Apple Inc. 的商标。在非 Apple 硬件上安装 macOS 可能违反 Apple 的软件许可协议，请自行承担风险。

**English**: This project is for learning, research and exchange only. It is not affiliated with Apple Inc. or HUAWEI. macOS is a trademark of Apple Inc. Installing macOS on non-Apple hardware may violate Apple's software license agreement. Use at your own risk.

## 致谢 / Credits

- [Acidanthera](https://github.com/acidanthera) — OpenCore, Lilu, VirtualSMC, AppleALC, VoodooPS2, NVMeFix, BrightnessKeys
- [WhateverXe](https://github.com/b00t0x/WhateverXe) — Intel Xe graphics support
- [OpenIntelWireless](https://github.com/OpenIntelWireless) — AirportItlwm, IntelBluetoothFirmware
- [VoodooI2C](https://github.com/VoodooI2C) — I2C trackpad support
- [USBToolBox](https://github.com/USBToolBox) — USB mapping
- [ECEnabler](https://github.com/1Revenger1/ECEnabler) — EC battery reading
- [CpuTscSync](https://github.com/lvs1974/CpuTscSync) — TSC synchronization
- All contributors and the Hackintosh community

## 许可证 / License

[MIT](LICENSE) © 2026 ziyuw-Plank
