## OCC 0.7.2 EFI r002
### config.plist
* Added *UEFI > Drivers* entry for **OpenCanopy.efi**
* Changed *UEFI > Audio > PlayChime* from *Auto* to *Disabled*
* Changed *Misc > PickerAttributes* from *1* to *17*
* Changed *Misc > PickerMod* from *Builtin* to *External*
* Changed *Misc > PickerVariant* from *Auto* to *daniele/BigSurFlat*

### Drivers
* Added **OpenCanopy.efi**

### Misc
* Added **[OpenCore Binary Data](https://github.com/acidanthera/OcBinaryData)** Resources
* Added **[BigSurFlat](https://github.com/82ghost82/BigSurFlat)**

## OCC 0.7.2 EFI r001
### config.plist
* Added *UEFI > AppleInput > GraphicsInputMirroring* (Boolean: *True*)

### Kexts
* Upgraded **AppleALC.kext** from 1.6.2 to 1.6.3
* Upgraded **Lilu.kext** from 1.5.4 to 1.5.5
* Upgraded **VirtualSMC.kext** from 1.2.5 to 1.2.6
* Upgraded **WhateverGreen.kext** from 1.5.1 to 1.5.2

### Drivers
* Upgraded **AudioDXE.efi**
* Upgraded **OpenRuntime.efi**

### Tools
* Upgraded all

## OC 0.7.1 EFI r001
### config.plist
* Added *ACPI > Quirks > SyncTableIds* (Boolean: *False*)
* Added *Kernel > Scheme > CustomKernel* (Boolean: *False*)

### Kexts
* Upgraded **AppleALC.kext** from 1.6.1 to 1.6.2
* Upgraded **Lilu.kext** from 1.5.3 to 1.5.4
* Upgraded **VirtualSMC.kext** from 1.2.4 to 1.2.5
* Upgraded **IntelMausi.kext** from 1.0.6 to 1.0.7
* Upgraded **NVMeFix.kext** from 1.0.8 to 1.0.9
* Upgraded **WhateverGreen.kext** from 1.5.0 to 1.5.1

## OC 0.7.0 EFI r002
### config.plist
* Changed *ACPI > Quirks > ResetLogoStatus* from *True* to *False* (reverted change from OC 0.7.0 EFI r001)

## OC 0.7.0 EFI r001
### config.plist
* Changed *ACPI > Quirks > ResetLogoStatus* from *False* to *True*
* Added *Kernel > Quirks > ProvideCurrentCpuInfo* (Boolean: *False*)
* Added *Misc > Entries > Item0 > Flavour* (String: *Auto*)
* Added *Misc > Security > AllowToggleSip* (Boolean: *False*)
* Added Flavour key to *Misc > Tools > Items*
* Changed *PlatformInfo > Generic > AdviseWindows* to *PlatformInfo > Generic > AdviseFeatures*
* Changed *UEFI > Output > GopPassThrough* from Boolean *False* to String *Disabled*
* Added *UEFI > ProtocolOverrides > AppleEg2Info* (Boolean: *False*)
      
### Kexts
* Upgraded **AppleALC.kext** from 1.6.0 to 1.6.1
* Upgraded **NVMeFix.kext** from 1.0.7 to 1.0.8
* Upgraded **VirtualSMC.kext** from 1.2.3 to 1.2.4
* Upgraded **WhateverGreen.kext** from 1.4.9 to 1.5.0

### Drivers
* Upgraded **HfsPlus.efi** (neglected to do this for OC 0.6.9)

## OC 0.6.9 EFI r002
### config.plist
* Removed: *ACPI > Patch* **_DSM -> XDSM** is unnecessary and therefore no longer included

### ACPI
* Restored missing **SSDT-PPMC.aml** (inadvertently deleted this when publishing 0C 0.6.9 EFI r001)

## OC 0.6.9 EFI r001
### config.plist
* Removed *ACPI > Patch* no longer includes **EC0 -> EC rename**
* Added *ACPI > Add* includes **SSDT-EC.aml** to inject Fake EC
* Added *UEFI > Quirks > EnableVectorAcceleration* (Boolean: *True*)
* Added *UEFI > Quirks > ForgeUefiSupport* (Boolean: *False*)
* Added *UEFI > ReloadOptionRoms* (Boolean: *False*)
* Changed *UEFI > AppleInput > CustomDelays* from String *Auto* to Boolean *False*

### ACPI
* Added: **SSDT-EC.aml** to inject Fake EC

### Kexts
* Upgraded **IntelMausi.kext** from 1.0.5 to 1.0.6
* Upgraded **VirtualSMC.kext** from 1.2.2 to 1.2.3
* Upgraded **NVMeFix.kext** from 1.0.6 to 1.0.7
* Upgraded **Lilu.kext** from 1.5.2 to 1.5.3
* Upgraded **AppleALC.kext** from 1.5.9 to 1.6.0

## OC 0.6.8 EFI r003
### config.plist
* Removed *ACPI > Patch* no longer includes **SAT0 -> SATA rename**
* Added *ACPI > Add* includes **SSDT-USBX.aml**

### ACPI
* Added **SSDT-USBX.aml** to inject the same power properties as **USBPorts.kext**

## OC 0.6.8 EFI r002
### config.plist
* Fixed errors identified by **OCValidate** tool:
  * OCS: Missing key Patch, context <Booter>!
  *  OCS: Missing key TextMode, context <Entries>!
  *  OCS: Missing key RealPath, context <Tools>!
  *  OCS: Missing key TextMode, context <Tools>!
  *  OCS: Missing key RealPath, context <Tools>!
  *  OCS: Missing key TextMode, context <Tools>!
  *  OCS: Missing key RealPath, context <Tools>!
  *  OCS: Missing key TextMode, context <Tools>!

## OC 0.6.8 EFI r001
### ACPI
* Added missing *_OSI("Darwin")* conditions to conditionally enable Apple devices for macOS:
     * **SSDT-DMAC**
     * **SSDT-PLUG**
     * **SSDT-PMCR**
     * **SSDT-PPMC**
     * **SSDT-XSPI**
### config.plist
* Added Base and BaseSkip keys to ACPI patches (ACPI > Patch)
* Added *Booter > Quirks > ForceBooterSignature* (Boolean: *False*)
* Added *UEFI > AppleInput* properties
* Added *UEFI > Output > GopPassThrough* (Boolean: *False*)
* Added *UEFI > Audio > ResetTrafficClass* (Boolean: *False*)
* Changed *UEFI > ProtocolOverrides > AppleEvent* to *UEFI > AppleInput*
* Changed *Kernel > Quirks > AppleCpuPmCfgLock* from *True* to *False* (only *Kernel > Quirks > AppleXcpmCfgLock* is required to be *True*)
### Kexts
* Upgraded **WhateverGreen.kext** from 1.4.8 to version 1.4.9
* Upgraded **VirtualSMC.kext** from 1.2.1 to version 1.2.2
* Upgraded **AppleALC.kext** from 1.5.8 to version 1.5.9
* Upgraded **NVMEfix.kext** from 1.0.5 to version 1.0.6
* Upgraded **Lilu.kext** from 1.5.1 to version 1.5.2


## OC 0.6.7 EFI r002 BETA
### config.plist
* Removed *ACPI > Add* no loger includes **SSDT-AWAC-HPET.aml**
* Added *ACPI > Add* includes **SSDT-AWAC-HPET-RTC.aml** (includes RTC patch)
* Removed *Kernel > Add* no longer includes **RTCMemoryFixup.kext**
* Changed *NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > boot-args* no longer includes **rtcfx_exclude**
### Kexts
* Removed **RTCMemoryFixup.kext**
### ACPI
* Removed **SSDT-AWAC-HPET.aml**
* Added **SSDT-AWAC-HPET-RTC.aml** which includes RTC patch (same as CLOVER's Fix RTC) which changes RTC memory length from 0x8 to 0x2 to prevent RTC memory corruption)

## OC 0.6.7 EFI r001
### config.plist changes
* Changed *Booter > Quirks > ProtectUefiServices* from *False* to *True*
* Added *PlatformInfo > Generic > MaxBIOSVersion* (Boolean: *False*)
* Added *Kernel > Add* entry for **NVMeFix.kext**
* Added *UEFI > Quirks > ActivateHpetSupport* (Boolean: *False*)
* Removed *UEFI > Input > KeyMergeThreshold*
* Changed *NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > boot-args* no longer includes **rtcfx_exclude=B0-B3,B7** but **rtcfx_exclude=B0-B8**. This change did not resolve the RTC issue for the G4 Mini.

### Kext Updates
* Changed default **USBPorts.kext** to match **USBPorts-NoHS14.kext**
* Upgraded **AppleALC.kext** from 1.5.7 to 1.5.8
* Upgraded **VirtualSMC.kext** from 1.2.0 to 1.2.1
* Upgraded **WhateverGreen.kext** from 1.4.7 to 1.4.8
* Added **NVMeFix.kext** 1.0.5

### Notes
This EFI update for Open Core 0.6.7 includes a **USBPorts.kext** that is equivalent to **USBPorts-NoHS14.kext** (also included in this EFI) to comply with the 15-port logical USB port limit (with *Kernel > Quirks > XhciPortLimit* (Boolean: *False*)).
This **USBPorts.kext** does not have port **HS14** (the internal Bluetooth USB port).
We can no longer boot with more than 15 logical USB ports starting with **Big Sur 11.3**.
This EFI also includes **USBPorts-16.kext** that you can edit to customize your own USB port map.
**USBPorts-16.kext** includes all 16 of the available logical USB ports on the G3, G4 and G5 Minis.

Before using this EFI, replace the following *PlatformInfo > Generic* values in *config.plist* with your own (use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)):
* MLB
* ROM
* SystemSerialNumber
* SystemUUID

This EFI enables the boot chime.
If you want OpenCore to load faster, disable *UEFI > AudioSupport* and delete the UEFI Audio driver (in *config.plist*).