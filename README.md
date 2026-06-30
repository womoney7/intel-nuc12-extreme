## Intel NUC12  Extreme hackintosh OpenCore EFI

![image](ScreenShot/NUC12.jpg)

### [简体中文](README.zh_CN.md)

### OpenCore

[OpenCore 1.0.7](https://github.com/acidanthera/OpenCorePkg)

### OS Version Tested

- macOS Monterey 12.x
- macOS Ventura  13.x
- macOS Sonoma   14.x
- macOS Sequoia  15.x
- macOS Tahoe    26.x

### Hardware

- Motherboard: Intel Corporation NUC12EDBv9
- Bios Version: EDADLMIV.0062.2024.0305.1535  03/05/2024
- CPU: Intel 12th i9-12900
- RAM: Lenovo 16GB（8GB*2） DDR4 3200Mhz
- SSD: KINGBANK KP230
- iGPU: Intel UHD Graphic 770 (Only work in Windows)
- GPU: AMD Radeon RX 6700 XT
- Audio: Intel Alder Point-S PCH - HD Audio
- Ethernet Card: Intel L225-LM
- Ethernet Card: Marvell AQtion 10Gbit
- WIFI: Intel Wi-Fi 6E AX211

### Notes

 - Use [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools) build your SMBIOS

### Tahoe 26.x Update Notes

- OpenCore config is validated with OpenCore 1.0.7 `ocvalidate`.
- `USBToolBox.kext` is updated to 1.2.0. `UTBMap.kext` is kept from the original machine-specific map.
- `XhciPortLimit` is temporarily enabled for Tahoe boot/install stability because the current USB map still exposes more than 15 ports. Rebuild the USB map later and turn this back off for a cleaner long-term setup.
- `AppleIGC.kext` is updated to 1.8 for the Intel I225-LM Ethernet controller.
- Marvell/Aquantia 10G Ethernet keeps `ForceAquantiaEthernet=true` and adds CaseySJ Aquantia kernel patches.
- Intel AX211 Wi-Fi is configured for the OCLP Modern Wi-Fi path:
  - `AirportItlwm.kext` 2.3.0 Ventura build
  - `IOSkywalkFamily.kext` replacement
  - `IO80211FamilyLegacy.kext` dependency
  - native `IOSkywalkFamily` blocked in `Kernel -> Block`
  - `IOName=pci14e4,43a0` spoof on `PciRoot(0x0)/Pci(0x14,0x3)`
- Intel Bluetooth uses `BlueToolFixup.kext`, `IntelBTPatcher.kext`, and `IntelBluetoothFirmware.kext`.
- OCLP/OCLP-Mod root patch is required after installation for Modern Wi-Fi. If patching fails during `kmutil`, remove incompatible third-party kexts from `/Library/Extensions` first.
- CPU display is patched with RestrictEvents `revpatch=sbvmm,cpuname` and `revcpuname=Intel Core i9-12900`. The About This Mac frequency is nominal display only; use `powermetrics` to check live turbo behavior.
- Debug boot arguments were removed for daily use. Re-add `-v keepsyms=1 debug=0x100` only when troubleshooting.

### Bios Setup

```
Security
   |-- Secure Boot
      |-- Secure Boot ：Disabled 
      
   |-- Security Features
      |-- Intel VT for Directed I/O (VT-d) ：Enabled
      
  I can't remember the other options ^_^
```



### Known Issues

NUC12 Extreme Restart after sleep （may be something wrong with the BIOS settings）

Intel Wi-Fi works with the current OCLP Modern Wi-Fi stack, but AWDL features such as AirDrop, Personal Hotspot, and Continuity may still be limited under Tahoe.

### Contact Us

QQ Group: 23304408

![image](ScreenShot/QRCode.png)


### Tools

- [Hackintool](https://github.com/headkaze/Hackintool) 
- [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools) AKA `OCAT`.
- [OpenCore Configurator](https://mackie100projects.altervista.org/opencore-configurator/) AKA `OCC`.
- [gibMacOS](https://github.com/corpnewt/gibMacOS) Build your own MacOS image.
- [ProperTree](https://github.com/corpnewt/ProperTree) Plist editor.
