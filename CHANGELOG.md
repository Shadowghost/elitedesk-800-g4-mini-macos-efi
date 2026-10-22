## OC 1.0.8 EFI r001

Upgrade from OpenCore 1.0.7 to 1.0.8. The [guide thread](https://www.insanelymac.com/forum/topic/343937-guide-catalina-big-sur-monterey-ventura-sonoma-sequoia-tahoe-on-hp-elitedesk-800-g4g5-mini-the-perfect-macmini81-hackintosh/) was used as reference.

### config.plist
* Added *UEFI > Drivers > Item n > HideVerbose* (Boolean: *False*)
* Removed *darkwake=2* from *NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > boot-args*
* Changed *Kernel > Add* entry *VoodooHDA.kext* to **VoodooHDA_BL.kext** (*ExecutablePath* *Contents/MacOS/VoodooHDA_BL*)
* Changed *Kernel > Add > AppleALC.kext > MaxKernel* from empty to *24.99.99*
* Added *Kernel > Add* entry for **IntelMausi.kext** (*MaxKernel* 18.99.99) for Ethernet on Mojave
* Changed *Kernel > Add > IntelMausiEthernet.kext > MinKernel* from *12.0.0* to *19.0.0*, since it requires Catalina or newer
* Changed *Booter > Patch* sample entry (*macOS to hacOS*, disabled) to an enabled *Skip Board ID check* patch, which lets *boot.efi* start macOS releases that no longer list *Macmini8,1* as supported (Tahoe)
* Added *ACPI > Add* entries for **SSDT-TPM-Off.aml**, **SSDT-MATH.aml**, **SSDT-FWHD.aml** and **SSDT-Other.aml**
* Added *ACPI > Patch* *TPM._STA -> TPM.XSTA* (paired with **SSDT-TPM-Off**), *MATH._STA -> MATH.XSTA* (paired with **SSDT-MATH**) and *RTC._CRS -> RTC.XCRS* (paired with **SSDT-AWAC-HPET-RTC**)
* Changed *ACPI > Patch* *_OSI -> XOSI*, *_WAK -> XWAK* and *_PTS -> XPTS* to only patch table *DSDT* with an exact *Count* (24, 1, 1), and *_PTS -> XPTS* to match *_PTS* with one argument
* Added *DeviceProperties > Add > pci-aspm-default* = *<02000000>* (ASPM L1) for SATA *Pci(0x17,0x0)*, both NVMe slots *Pci(0x1B,0x0)/Pci(0x0,0x0)* and *Pci(0x1B,0x4)/Pci(0x0,0x0)*, audio *Pci(0x1F,0x3)* and Ethernet *Pci(0x1F,0x6)*; the audio property was also added to every *EXTRAS/DeviceProperties* variant
* Added *igfxfw=2* to *NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > boot-args* (load Apple's GuC firmware for the UHD 630; requires Intel ME enabled)
* Validates cleanly against **ocvalidate** 1.0.8

### ACPI
* **SSDT-AWAC-HPET-RTC.aml**: keep the original *RTC* device and only replace its *_CRS* (RTC memory length 0x02) instead of disabling it and adding *RTC0*; *STAS* changed from 2 to 1 accordingly (no functional change)
* **SSDT-DMAC**, **SSDT-EC**, **SSDT-PMCR**, **SSDT-PPMC**, **SSDT-USBX**, **SSDT-WAK**, **SSDT-XOSI**, **SSDT-XSPI**: removed redundant *Else* branches (functionally identical)
* Added **SSDT-TPM-Off.aml**: hides the TPM device from macOS; only takes effect when TPM is enabled in BIOS (e.g. for Windows 11 dual boot)
* Added **SSDT-MATH.aml**: enables the *MATH* (coprocessor) device under macOS (cosmetic)
* Added **SSDT-FWHD.aml**: adds the firmware hub device *FWHD* under macOS (cosmetic)
* Added **SSDT-Other.aml**: names the unnamed PCI devices *@14,2* (*SRAM*) and *@12* (*THMC*) under macOS (cosmetic)

### Kexts
* Replaced **VoodooHDA.kext** 3.6.5 with **VoodooHDA_BL.kext** 3.7.0-alpha (b320ac9). The previous build linked against *IOAudioFamily* and *IOGraphicsFamily*, which OpenCore cannot link against on Tahoe, so it could not be injected there and Tahoe had no audio. The bootloader build embeds *IOAudioFamily* and only needs kernel KPIs and *IOPCIFamily*, so it can be injected with SIP fully enabled. Side effect: Apple's *IOAudioFamily* (and therefore *AppleUSBAudio*) does not load on Tahoe
* Changed **VoodooHDA_BL.kext** *Info.plist*: *VoodooHDAEnableVolumeChangeFix* = *true* in both personalities (needed for volume control on this machine), and *IONameMatch* = *HDAS*, *HDEF* so matching does not depend on whether the controller has been renamed to *HDEF*
* Added **IntelMausi.kext** 1.0.8 (Acidanthera) for Ethernet on Mojave, where **IntelMausiEthernet.kext** 3.x does not work. Together with the kernel ranges above this makes the EFI support Mojave 10.14.6
* Upgraded **AppleALC.kext** from 1.9.7 to 1.9.8
* Upgraded **IntelMausiEthernet.kext** from 3.0.0 (build 3.0.3) to 3.0.1 (fixes initialisation under Tahoe with AppleVTD enabled)
* Upgraded **VirtualSMC.kext** from 1.3.7 to 1.3.8
* Upgraded **WhateverGreen.kext** from 1.7.0 to 1.7.1

### Drivers
* Upgraded **AudioDxe.efi**, **OpenCanopy.efi**, **OpenRuntime.efi**, **ResetNvramEntry.efi**

### Tools
* Upgraded **CleanNvram.efi**, **ControlMsrE2.efi**, **OpenShell.efi**

### Resources
* Updated **[OpenCore Binary Data](https://github.com/acidanthera/OcBinaryData)** (adds *OpticalDrive* icons to the Acidanthera themes, new *ExtHardDrive* icon for *Chardonnay*)
* Added **[GoldenGate Extended](https://github.com/HJebbour/GoldenGateExt-OpenCore-Theme)** 3.0 as alternative theme (*Resources/Image/Acidanthera/GoldenGateExt*, select with *PickerVariant* *Acidanthera/GoldenGateExt*), completed with the *FirmwareSettings*, *NetworkBoot* and *OpticalDrive* icons from the stock **GoldenGate** theme
* **BigSurFlat** (default theme) has no upstream changes since January 2022; the bundled copy is identical apart from the existing *Shutdown.icns* -> *ShutDown.icns* rename

### Notes
* Unchanged, already the latest release: **Lilu.kext** 1.7.2, **FeatureUnlock.kext** 1.1.8, **NVMeFix.kext** 1.1.3, **RestrictEvents.kext** 1.1.6, **HfsPlus.efi**
* README: documented the boot picker themes and the alternative audio solutions for macOS Tahoe (regular **VoodooHDA.kext**, *HDAUniversal*, *SimpleHDA*, OCLP)

## OC 1.0.7 EFI r001

Upgrade from OpenCore 0.8.9 to 1.0.7, covering all intermediate releases (0.9.0 - 1.0.7).

### config.plist
* Added *Booter > Quirks > ClearTaskSwitchBit* (Boolean: *False*)
* Added *Booter > Quirks > FixupAppleEfiImages* (Boolean: *True*)
* Added *Kernel > Quirks > DisableIoMapperMapping* (Boolean: *False*)
* Added *Misc > Boot > InstanceIdentifier* (String: empty)
* Added *Misc > Serial > Init* (Boolean: *False*)
* Added *Misc > Serial > Override* (Boolean: *False*)
* Added *UEFI > Drivers > Item n > Comment* (String: empty)
* Added *UEFI > Output > ConsoleFont* (String: empty)
* Added *UEFI > Output > GopBurstMode* (Boolean: *False*)
* Added *UEFI > Output > InitialMode* (String: *Auto*)
* Added *UEFI > Output > ReconnectGraphicsOnConnect* (Boolean: *False*)
* Added *UEFI > Output > UIScale* (Integer: *1*)
* Added *UEFI > ProtocolOverrides > PciIo* (Boolean: *False*)
* Added *UEFI > Quirks > EnableVmx* (Boolean: *False*)
* Added *UEFI > Quirks > ShimRetainProtocol* (Boolean: *False*)
* Added *UEFI > Unload*
* Changed *Kernel > Quirks > ThirdPartyDrives* from *false* to *true*
* Changed *Misc > Security > SecureBootModel* from *Default* to *Disabled* (required to install/upgrade Sonoma 14.4+, Sequoia and Tahoe)
* Changed *Kernel > Add* entry *IntelMausi.kext* to **IntelMausiEthernet.kext**
* Changed *Kernel > Add* entry *EFICheckDisabler.kext* to **RestrictEvents.kext**
* Added *NVRAM > Add > 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102 > revpatch* (String: *sbvmm*, paired with **RestrictEvents.kext**, enables OTA updates)
* Added *NVRAM > Delete > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > csr-active-config* so the configured SIP value is enforced on every boot
* Moved *NVRAM > Add > 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 > UIScale* to *UEFI > Output > UIScale* (setting both is rejected by **ocvalidate**)
* Added *Kernel > Add* entry for **VoodooHDA.kext** (enabled, *MinKernel* 25.0.0), so audio works on every supported macOS without any manual edit: **AppleALC.kext** serves Catalina to Sequoia, **VoodooHDA.kext** serves Tahoe
* Changed *Kernel > Add* comments on **AppleALC.kext** and **VoodooHDA.kext** to document that version split
* Changed *Misc > Boot > PickerAttributes* from *17* to *145* to set *OC_ATTR_USE_FLAVOUR_ICON*, so the boot picker uses the theme's per-OS icons (Windows 10, Windows 11, Linux)
* Fixed stray trailing space in the *Booter > Quirks > ResizeAppleGpuBars* key name (the quirk was never applied before)
* Fixed stray trailing space in the *Misc > Debug > LogModules* key name
* Reordered all keys to match *Sample.plist*; validates cleanly against **ocvalidate** 1.0.7

### ACPI
* **SSDT-PTS.aml**: changed method *_PTS* from *Serialized* to *NotSerialized* (bug fix)

### Kexts
* Added **RestrictEvents.kext** 1.1.6
* Added **VoodooHDA.kext** 3.6.5 ([CloverHackyColor](https://github.com/CloverHackyColor/VoodooHDA), GPLv2) for audio under macOS Tahoe, where Apple removed *AppleHDA*. Enabled, but limited to *MinKernel* 25.0.0 so it is never injected on Sequoia or older, where it would compete with *AppleHDAController* for the HDA PCI device. **AppleALC.kext** needs no matching *MaxKernel*: it patches *AppleHDA*, so on Tahoe it is inert and never binds the device VoodooHDA claims. SIP stays fully enabled because OpenCore injects the kext rather than installing it to */Library/Extensions*. Users who restore *AppleHDA* on Tahoe via OCLP should disable **VoodooHDA.kext**
* Added **IntelMausiEthernet.kext** 3.0.3 (Mieze) - replaces Acidanthera's *IntelMausi.kext*, which does not support Ethernet under macOS Tahoe
* Removed **EFICheckDisabler.kext** (no longer needed with **RestrictEvents.kext**)
* Removed **IntelMausi.kext**
* Upgraded **AppleALC.kext** from 1.7.9 to 1.9.7
* Upgraded **FeatureUnlock.kext** from 1.1.3 to 1.1.8
* Upgraded **Lilu.kext** from 1.6.3 to 1.7.2
* Upgraded **NVMeFix.kext** from 1.1.0 to 1.1.3
* Upgraded **VirtualSMC.kext** from 1.3.0 to 1.3.7
* Upgraded **WhateverGreen.kext** from 1.6.4 to 1.7.0
* Added *usb-port-type* and *usb-port-number* to every port definition in **USBPorts.kext**, **USBPorts-16.kext** and **USBPorts-noHS14.kext** for macOS Tahoe compatibility (one USB map now works for Tahoe and earlier)

### Drivers
* Upgraded **AudioDxe.efi**, **HfsPlus.efi**, **OpenCanopy.efi**, **OpenRuntime.efi**, **ResetNvramEntry.efi**

### Tools
* Upgraded **CleanNvram.efi**, **ControlMsrE2.efi**, **OpenShell.efi**

### Resources
* Updated **[OpenCore Binary Data](https://github.com/acidanthera/OcBinaryData)** (adds *FirmwareSettings* and *NetworkBoot* labels/icons and the *Terminus* console fonts)

### Notes
* This EFI keeps *NVRAM > Add > csr-active-config* = `<00000000>` (SIP fully enabled), and it stays valid when enabling the bundled **VoodooHDA.kext**, since OpenCore injects it. Only a */Library/Extensions* install (`<01000000>`) or OpenCore Legacy Patcher (`<03080000>`) would need it relaxed.
* The **BigSurFlat** theme has no *Other.icns*; neither do Acidanthera's own themes, so entries that are neither macOS, Windows nor a tool fall back to the generic *HardDrive* icon. Purely cosmetic.
* **SSDT-TPM-Off**, **SSDT-MATH** and **SSDT-FWHD** are not included, because they depend on device paths in the machine's own DSDT that could not be verified. *SSDT-MATH* and *SSDT-FWHD* are purely cosmetic; *SSDT-TPM-Off* only matters when TPM is enabled in BIOS for Windows 11 dual boot.

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