# Hackintosh: MSI MAG B460 Tomahawk

![Check](https://img.shields.io/badge/Status-Pass-brightgreen)
![GitHub issues](https://img.shields.io/github/issues/theofficialcopypaste/ASRockB460MSL-OC?color=blue&label=Issues)
[![Bootloader](https://img.shields.io/badge/Bootloader-OpenCore-yellow)](https://github.com/theofficialcopypaste/ASRockB460MSL-OC/releases)
[![MOnterey](https://img.shields.io/badge/Compatible-Monterey-purple)](https://www.apple.com/ge/macos/monterey/)
[![Ventura](https://img.shields.io/badge/Compatible-Ventura-orange)](https://www.apple.com/my/macos/ventura/)
[![Version](https://img.shields.io/badge/Version-0.8.7-white)](https://github.com/acidanthera/OpenCorePkg/releases)

## Table of Content

- [Introduction](#introduction)
- [Devices Specification](#devices-specification)
- [EFI](#efi)
  - [ACPI](#acpi)
  - [Booter](#booter)
  - [DeviceProperties](#deviceproperties)
  - [Kernel](#kernel)
  - [Misc](#misc)
  - [PlatformInfo](#platforminfo)
  - [UEFI](#uefi)
- [Changelog](#changelog)
- [Tips](#tips)
- [Results](#results)
- [Credits](#credits)
  
### Introduction

This is my current EFI clone that I built according to my hardware. Feel free to read my content. If you have a similar build but different settings, you might consider checking this out. Before read, below is the best way to checkout the latest OpenCore guide and news

- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) "Getting Started"
- Checkout latest [post](https://dortania.github.io), news and update directly from developer
  
### Devices Specification

- Hardware and Differences
  
  | **Hardware** | **Real iMac** | **My Hack** |
  | --- | --- | --- |
  | **Motherboard** | Apple Custom | MSI MAG B460 Tomahawk |
  | **Processor** | [Intel® Core™ i5-10500](https://ark.intel.com/content/www/us/en/ark/products/199277/intel-core-i510500-processor-12m-cache-up-to-4-50-ghz.html) | [Intel® Core™ i5-10400](https://ark.intel.com/content/www/us/en/ark/products/199271/intel-core-i510400-processor-12m-cache-up-to-4-30-ghz.html) |
  | **Series** | 10th Gen | 10th Gen |
  | **Code Name** | Comet Lake | Comet Lake |
  | **Socket** | LGA1200 | LGA1200 |
  | **Core** | 6   | 6   |
  | **Thread** | 12  | 12  |
  | **Base Freq** | 3.1 GHz | 2.9 GHz |
  | **Turbo Boost** | 4.5 GHz | 4.3 GHz |
  | **ROM / FW Type** | EFI | EFI / Legacy |
  | **T2 Sec. Chip** | Yes | No |
  | **RAM** | Up to 2666 MHz DDR4 SDRAM | Up to 2666 MHz DDR4 SDRAM |
  | **iGPU** | [Intel UHD 630](https://ark.intel.com/content/www/us/en/ark/products/graphics/126790/intel-uhd-graphics-630.html) | [Intel UHD 630](https://ark.intel.com/content/www/us/en/ark/products/graphics/126790/intel-uhd-graphics-630.html) |
  | **dGPU** | [Radeon Pro 5300 4GB](https://www.amd.com/en/products/graphics/amd-radeon-rx-5300), TB | [MSI RX 5500 XT MECH OC 4GB](https://www.msi.com/Graphics-Card/Radeon-RX-5500-XT-MECH-4G-OC), HDMI & DP |
  | **Native Resolution** | 5120 x 2880 | 5120 x 2880 |
  | **Firewire Ports** | None | None |
  | **Expansion Slot** | SDXC SD Card | Upgradeable |
  | **Wi-Fi** | 802.11ac (Broadcom) | 802.11ac (Broadcom) |
  | **Bluetooth** | 5.0 | 5.0 via BCM94360CD |
  | **Standard Storage** | 256 GB SSD | Upgradeable |

- Suggested Hardware to copy iMac20,1
  - Processors
	- [Intel® Core™ i5-10500 Processor](https://ark.intel.com/content/www/us/en/ark/products/199277/intel-core-i510500-processor-12m-cache-up-to-4-50-ghz.html)
  - Motherboard
	- B5XX or Z5XX or any 11th/10th Gen Motherboard. ie: [Gigabyte Vision Z590](https://www.gigabyte.com/Motherboard/Z590-VISION-D-rev-10#kf)
  - RAM
	- Any equivalent DDR4 2666/2400/2133 MHz memory modules (recommend 2666 Mhz)
  - Storage
	- Any equivalent NVMe with PCIe 3.0 Support ie: [WD Black SN750](https://www.westerndigital.com/en-ap/products/internal-drives/wd-black-sn750-se-nvme-ssd#WDS500G1B0E)
  - Wi-Fi
	- [Fenvi T919](https://www.fenvi.com/product_detail_16.html), BCM94360CD
  - GPU
	- [MSI RX 5500 XT MECH OC 4GB](https://www.msi.com/Graphics-Card/Radeon-RX-5500-XT-MECH-4G-OC)
  
### EFI

- Structure
  
```zsh
EFI
├── BOOT 
│   └── BOOTx64.efi                     // Modern BIOS firmware
└── OC 
	├── ACPI                            // ACPI Library
	│   └── SSDT-EXT.aml
	├── config.plist                    // OpenCore configuration
	├── Drivers                         // Drivers Library
	│   ├── HfsPlus.efi             
	│   ├── OpenCanopy.efi             
	│   ├── OpenRuntime.efi          
	│   └── ResetNvramEntry.efi     
	├── Kexts                           // Kernel extensions
	│   ├── AppleALC.kext    
	│   ├── IntelMausi.kext         
	│   ├── Lilu.kext                 
	│   ├── LucyRTL8125Ethernet.kext  
	│   ├── SMCProcessor.kext       
	│   ├── SMCSuperIO.kext         
	│   ├── USBMap.kext          
	│   ├── VirtualSMC.kext         
	│   └── WhateverGreen.kext       
	├── OpenCore.efi 
	├── Resources                       // Binary Resource
	│   ├── Audio                     
	│   ├── Font                 
	│   ├── Image                  
	│   │   └── Acidanthera             // Theme Library
	│   │       ├── Chardonnay 
	│   │       ├── GoldenGate 
	│   │       └── Syrah 
	│   └── Label
	└── Tools
```

> **Note**: Get OC binary resource and additional EFI drivers [here](https://github.com/acidanthera/OcBinaryData)

#### ACPI

- [SSDT-EXT](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/SSDT/SSDT-EXT.dsl) contain:
  
  - ALSD
	- Ambient Light Sensor attached to `AppleLMUController` (ACPI0008)
  - AWAC
	- System clock fix on Z390, B460, Z490 motherboards
  - EC
	- Fake Embedded Controller as an alternative EC controller, also prevents actual `AppleACPIEC` from being loaded on macOS
  - DRAM
	- Dynamic Random Access Memory 
  - PXSX
	- Patched PCI Bridge for GFX0
  - PGMM
	- Processor Gaussian Mixture Model
  - SBUS
	- Patched System BUS
  - TSUB
	- Thermal Subsystem
  - USBX
	- Patched USB Power Management

#### Booter

- Quirks
  
| Input | Type | Value |
| --- | --- | --- |
| AvoidRuntimeDefrag | boolean | Yes |
| DevirtualiseMmio | boolean | Yes |
| EnableSafeModeSlide | boolean | Yes |
| ProvideCustomSlide | boolean | Yes |
| SyncRuntimePermissions | boolean | Yes |
| ProvideMaxSlide | number | 0   |
| ResizeAppleGpuBars | number | -1  |

> **Note**: Other than above is `No`

#### DeviceProperties

Please see the entire patch [here](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/DeviceProperties/deviceproperties.plist).

> **Note**: The format is in XML.

#### Kernel

- Kernel extension used for this build.
  
| Kext | Details |
| --- | --- |
| **AppleALC** | An open source kernel extension enabling native macOS HD audio for not officially supported codecs without any filesystem modifications. AppleALCU can be used for systems with digital-only audio. |
| **IntelMausi** | Intel onboard LAN driver for macOS |
| **Lilu** | An open source kernel extension bringing a platform for arbitrary kext, library, and program patching throughout the system for macOS. |
| **LucyRTL8125Ethernet** | A macOS driver for Realtek RTL8125 2.5GBit Ethernet Controllers |
| **USBMap** | USB Port configuration. Require [USBMap](https://github.com/corpnewt/USBMap) or [USBToolbox](https://github.com/USBToolBox/tool) |
| **VirtualSMC** | Advanced Apple SMC emulator in the kernel. Requires [Lilu](https://github.com/acidanthera/Lilu) for full functioning. Include with SMCProcessor & SMCSuperIO. |
| **Whatevergreen** | [Lilu](https://github.com/acidanthera/Lilu) plugin providing patches to select GPUs on macOS. Requires Lilu 1.5.6 or newer. |

- Patch

| Input | Type | Value |
| --- | --- | --- |
| Arch | string | x86_64 |
| Base | string |     |
| Comment | string | Enable TRIM for SSD \\| Catalina + |
| Count | number | 0   |
| Enabled | boolean | Yes |
| Find | data | 00415050 4C452053 534400 |
| Identifier | string | com.apple.iokit.IOAHCIBlockStorage |
| Limit | number | 0   |
| Mask | data |     |
| MaxKernel | string |     |
| MinKernel | string |     |
| Replace | data | 00000000 00000000 000000 |
| ReplaceMask | data |     |
| Skip | data |     |

- Quirks
  
| Input | Type | Value |
| --- | --- | --- |
| AppleXcpmCfgLock | boolean | Yes |
| DisableIoMapper | boolean | Yes |
| PanicNoKextDump | boolean | Yes |
| PowerTimeoutKernelPanic | boolean | Yes |
| SetApfsTrimTimeout | number | 0   |

> **Note**: Other than above is `No`

#### Misc

| Input | Type | Value |
| --- | --- | --- |
| ConsoleAttributes | boolean | Yes |
| HibernateMode | boolean | Yes |
| HideAuxiliary | string | Auto |
| LauncherOption | string | Full |
| LauncherPath | string | Default |
| PickerAttributes | number | 147 |
| PickerMode | string | External |
| PickerVariant | string | Acidanthera\GoldenGate |
| ShowPicker | boolean | Yes |
| TakeoffDelay | number | 0   |
| Timeout | number | 5   |

> **Note**: Other than above is `No`

#### PlatformInfo

- SMBIOS: [iMac20,1](https://everymac.com/systems/apple/imac/specs/imac-core-i5-3.1-6-core-27-inch-retina-5k-2020-specs.html)
  
#### UEFI

- APFS
  
| Input | Type | Value |
| --- | --- | --- |
| EnableJumpstart | boolean | Yes |
| HideVerbose | boolean | Yes |
| MinDate | number | 0   |
| MinVolume | number | 0   |

- Drivers
  
| Input | Type | Value |
| --- | --- | --- |
| Enable | boolean | Yes |
| Path | boolean | HFSPlus.efi |

| Input | Type | Value |
| --- | --- | --- |
| Enable | boolean | Yes |
| Path | boolean | OpenRuntime.efi |

| Input | Type | Value |
| --- | --- | --- |
| Enable | boolean | Yes |
| Path | boolean | HFSPlus.efi |

| Input | Type | Value |
| --- | --- | --- |
| Enable | boolean | Yes |
| Path | boolean | ResetNvramEntry.efi |

- Input

| Input | Type | Value |
| --- | --- | --- |
| KeyForgetThreshold | number | 5   |
| LeySupport | boolean | Yes |
| KeySupportMode | boolean | Auto |
| PointerSupportMode | string | ASUS |
| TimerResolution | number | 50000 |

> **Note**: Other than above is `No`

- Output

| Input | Type | Value |
| --- | --- | --- |
| GopPassThrough | string | Disable |
| ProvideConsoleGop | boolean | Yes |
| Resolution | string | max |
| TextRenderer | string | BuiltinGraphics |
| UIScale | number | -   |

> **Note**: Other than above is `No`

- ProtocolOverrides

| Input | Type | Value |
| --- | --- | --- |
| FirmwareVolume | boolean | Yes |

> **Note**: Other than above is `No`

Quirks

| Input | Type | Value |
| --- | --- | --- |
| EnableVectorAcceleration | boolean | Yes |
| ExitBootServicesDelay | number | 0   |
| RequestBootVarRouting | boolean | Yes |
| ResizeGpuBars | number | -1  |
| TscSyncTimeout | number | 0   |

> **Note**: Other than above is `No`

## Changelog

**[0.8.8](https://github.com/acidanthera/OpenCorePkg/releases)**

- Added Linux support to QemuBuild.command used for Duet debugging
- Added prebuilt mtoc universal binary with Apple Silicon support
- Added SD card device path support for boot device selection
- Added `.contentVisibility` to hide and disable boot entries
- Built in new secure PE/COFF loader
- Corrected OpenDuet build on Apple Silicon
- Fixed intermittent prelinking failures caused by XML corruption when kext blocking is enabled
- Fixed `Kernel` -> `Block` entries not being processed if one was skipped due to `Arch`
- Removed magic Acidanthera sequence from OpenCore files used for picker hiding
- Updated AppleKeyboardLayouts.txt from macOS 13.1
- Updated builtin firmware versions for SMBIOS and the rest
- Updated ocvalidate to allow duplicate tool if FullNvramAccess is different
- Updated underlying EDK II package to edk2-stable202211

## My Build

- [Release](https://github.com/theofficialcopypaste/MSIB460Tomahawk/releases)

> **Note**: SMBIOS is censored by [OC-Anonymizer](https://github.com/dreamwhite/OC-Anonymizer). This will include OC config `v0.8.7`, `v0.8.8` and `v0.8.8 debug`

## Patches

- permanent `agdpmod=pikera` via IGPU DeviceProperties.
- permanent `acpi-wake-type` via XHCI and PXSX DeviceProperties.
- patched `ATY,Python` FB for MSI RX 5500 XT Mech OC 4GB.
- less acpi code

## Tips

- [Ambient Light Sensor Enable](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/Tips/Ambient%20Light%20Sensor%20Enable/Ambient%20Light%20Sensor.md)
- [Boot Arg to Properties](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/Tips/Boot%20Arg%20to%20Properties/Boot%20Arg%20to%20Properties.md)
- [Missing Bridge](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/Tips/Missing%20Bridge/Missing%20Bridge.md)
- [Rename Devices](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/Tips/Rename%20Devices/Rename%20Devices.md)
- [USB keyboard Wake](https://github.com/theofficialcopypaste/MSIB460Tomahawk/blob/main/Tips/USB%20Keyboard%20Wake/USB%20Keyboard%20Wake.md)

## Results

- Info

![Screenshot 2023-01-08 at 10 57 39 PM](https://user-images.githubusercontent.com/72515939/211203649-80fb3ce1-4f21-4568-95ce-e10ac404b876.png)

- Devices

![Screenshot 2023-01-08 at 11 02 36 PM](https://user-images.githubusercontent.com/72515939/211203652-b424c0f0-57f3-4ed5-b83a-113144d025c2.png)
![Screenshot 2023-01-08 at 11 02 44 PM](https://user-images.githubusercontent.com/72515939/211203655-ad4ba3c9-7873-4b56-b49a-6b603c019e28.png)

- Mapped USB

![Screenshot 2023-01-08 at 11 03 44 PM](https://user-images.githubusercontent.com/72515939/211203659-fd068e4f-41bb-4c0a-b694-f06f4b5737ff.png)

- Multiboot Capable? **Yes!**

![Screenshot 2023-01-08 at 11 42 52 PM](https://user-images.githubusercontent.com/72515939/211205718-a1962bcc-c1c9-4999-b44f-70be349aa870.png)
![Screenshot_20230108_234715](https://user-images.githubusercontent.com/72515939/211205948-3b8cc5a3-5d7e-4812-b18f-8699d91c6db3.png)

## Credits

#### [Acidanthera](https://github.com/acidanthera) | [Dortania](https://github.com/dortania) | [dreamwhite](https://github.com/dreamwhite)
