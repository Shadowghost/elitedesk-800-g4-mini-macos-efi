## OC 0.8.9 EFI r001

### config.plist
* Changed *ACPI > Quirks > RebaseRegions* from *true* to *false*
* Changed *ACPI > Quirks > NormalizeHeaders* from *true* to *false*
* Changed *Booter > Quirks > ProtectUefiServices* from *true* to *false*
* Changed *Kernel > Quirks > LapicKernelPanic* from *false* to *true*
* Changed *UEFI > APFS > MinDate* from *0* to *-1* to allow booting all supported versions of macOS
* Changed *UEFI > APFS > MinVersion* from *0* to *-1* to allow booting all supported versions of macOS
* Added *ACPI > Add* for **SSDT-PTS.aml**
* Added *ACPI > Add* for **SSDT-WAK.aml**
* Added *ACPI > Patch* to rename *_PTS_* to *XPTS* (paired with **SSDT-PTS**)
* Added *ACPI > Patch* to rename *_WAK* to *XWAK* (paired with **SSDT-WAK**)
* Added *Kernel > Block > Item n > Strategy* (String: Disable)
* Added *Kernel > Quirks > CustomPciSerialDevice* (Boolean: *False*)
* Added *Kernel > Quirks > ForceAquantiaEthernet* (Boolean: *False*)
* Added *Misc > Boot > HibernateSkipsPicker* (Boolean: *False*)
* Added *Misc > Debug > LogModules* (String: *)
* Added *Misc > Serial*
* Added *Misc > Tools > Item n > FullNvramAccess* (Boolean: *False*)
* Added *NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > ForceDisplayRotationInEFI* (Integer: 0)
* Added *NVRAM > Delete > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > ForceDisplayRotationInEFI*
* Added *UEFI > AppleInput > PointerDwellClickTimeout* (Integer: 0)
* Added *UEFI > AppleInput > PointerDwellDoubleClickTimeout* (Integer: 0)
* Added *UEFI > AppleInput > PointerDwellRadius* (Integer: 0)
* Added *UEFI > AppleInput > PointerPollMin* (Integer: 10)
* Added *UEFI > AppleInput > PointerPollMax* (Integer: 80)
* Added *UEFI > AppleInput > PointerPollMask* (Integer: -1)
* Added *UEFI > Audio > AudioOutMask* (Integer: 1)
* Added *UEFI > Audio > DisconnectHda* (Boolean: *False*)
* Added *UEFI > Audio > MaximumGain* (Integer: -15)
* Added *UEFI > Audio > MinimumAssistGain* (Integer: -30)
* Added *UEFI > Audio > MinimumAudibleGain* (Integer: -55)
* Added *UEFI > Drivers > Item 3* (ResetNvramEntry.efi)
* Added *UEFI > Drivers > Item n > LoadEarly* (Boolean: *False*)
* Added *UEFI > Drivers > ResetNvramEntry.efi > Arguments* (String: --preserve-boot)
* Removed *Misc > Debug > SerialInit*
* Removed *Misc > Security > AllowNvramReset*
* Removed *Misc > Security > AllowToggleSip*
* Removed *NVRAM > LegacyEnable*
* Removed *UEFI > Audio > AudioOut*
* Removed *UEFI > Audio > MinimumVolume*
* Removed *UEFI > Audio > VolumeAmplifier*

### ACPI
* Added **SSDT-WAK.aml** to handle potential corrupt Arg0 on wake from sleep (Rehabman's _WAK fix)
* Updated **SSDT-XOSI.aml** to include Windows 11 (Windows 2021)

### Kexts
* Upgraded **AppleALC.kext** from 1.6.7 to 1.7.8
* Upgraded **FeatureUnlock.keyt** from 1.0.4 to 1.1.3
* Upgraded **Lilu.kext** from 1.5.8 to 1.6.3
* Upgraded **NVMeFix.kext** from 1.0.9 to 1.1.0
* Upgraded **VirtualSMC.kext** from 1.2.8 to 1.3.0
* Upgraded **WhateverGreen.kext** from 1.5.5 to 1.6.3

### Drivers
* Added **ResetNvramEntry.efi**
* Upgraded **AudioDxe.efi**
* Upgraded **HfsPlus.efi**
* Upgraded **OpenRuntime.efi**

### Tools
* Upgraded **CleanNvram.efi**
* Upgraded **ControlMsrE2.efi**
* Upgraded **OpenShell.efi**

### Resources
* Upgrade BugSurFlat theme

## OC 0.7.6 EFI r001

### config.plist
* Added *Kernel > Add* entry for *FeatureUnlock.kext* (disabled)
* Added *Booter > Quirks > ResizeAppleGpuBars* (Integer: *-1*)
* Added *NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > boot-args > igfxfw=2*
* Added *UEFI > Quirks > ResizeGpuBars* (Integer: *-1*)
* Added *UEFI > Output > ReconnectGraphicsOnConnect* (Boolean: *False*)
* Added *UEFI > Output > UIScale* (Integer: *0*)
* Added *UEFI > Quirks > EnableVmx* (Boolean: *False*)
* Removed *NVRAM > Add > 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 > UIScale*

### Kexts
* Added **FeatureUnlock.keyt** 1.0.4
* Upgraded **AppleALC.kext** from 1.6.5 to 1.6.7
* Upgraded **Lilu.kext** from 1.5.6 to 1.5.8
* Upgraded **VirtualSMC.kext** from 1.2.7 to 1.2.8
* Upgraded **WhateverGreen.kext** from 1.5.4 to 1.5.5

### Drivers
* Upgraded **HfsPlus.efi**
* Upgraded **OpenCanopy.efi**

## OC 0.7.4 EFI r001

### Kexts
* Upgraded **AppleALC.kext** from 1.6.4 to 1.6.5
* Upgraded **WhateverGreen.kext** from 1.5.3 to 1.5.4

### Tools
* Upgraded **OpenShell.efi**

## OC 0.7.3 EFI r001
### config.plist
* Added *UEFI > Quirks > ForceOcWriteFlash* (Boolean: *False*)
* Copy new *UEFI > Drivers* structure from Sample.plist (Path, Enabled, Arguments)
* Switched *DeviceProperties* to minimal config based on [3xDP](EXTRAS/DeviceProperties/3xDP.plist)

### Kexts
* Upgraded **AppleALC.kext** from 1.6.3 to 1.6.4
* Upgraded **Lilu.kext** from 1.5.5 to 1.5.6
* Upgraded **VirtualSMC.kext** from 1.2.6 to 1.2.7
* Upgraded **WhateverGreen.kext** from 1.5.2 to 1.5.3

### Drivers
* Upgraded **AudioDXE.efi**
* Upgraded **OpenCanopy.efi**
* Upgraded **OpenRuntime.efi**

### Tools
* Upgraded all

## OC 0.7.2 EFI r002
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

## OC 0.7.2 EFI r001
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