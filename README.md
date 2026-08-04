## HP EliteDesk 800 G4 Mini OpenCore EFI

[![latest release](https://img.shields.io/github/v/release/Shadowghost/elitedesk-800-g4-mini-macos-efi?label=latest%20release)](https://github.com/Shadowghost/elitedesk-800-g4-mini-macos-efi/releases)

This repository contains a generic [OpenCore](https://github.com/acidanthera/OpenCorePkg) based **EFI folder** for booting macOS on **HP EliteDesk 800 G4 Mini** computers.

This EFI is based off the excellent work of [deeveedee](https://www.insanelymac.com/forum/profile/1099364-deeveedee/) over at [insanelymac](https://www.insanelymac.com/forum/topic/343937-guide-catalina-big-sur-monterey-ventura-sonoma-sequoia-tahoe-on-hp-elitedesk-800-g4g5-mini-the-perfect-macmini81-hackintosh/).

For more information on how to install macOS on a **HP EliteDesk 800 G4 Mini** please consult the linked thread above.

For convenience sake this repository includes the recommended UEFI configuration and additional *DeviceProperties* configurations in the [EXTRAS folder](EXTRAS).

Currently based on **OpenCore 1.0.7**, covering macOS Catalina through Tahoe.

Features of this EFI:

* Working macOS boot
* Working BootCamp
* Graphical boot picker via OpenCanopy with [BigSurFlat theme](https://github.com/82ghost82/BigSurFlat)
* 15 port limit of BigSur is complied with by disabling the internal ports for the WiFi/BT module
* USB port map is compatible with both macOS Tahoe and earlier releases
* Per-OS boot entry icons (Windows 10, Windows 11, Linux) via OpenCanopy flavours
* Working audio on every supported macOS: AppleALC up to Sequoia, VoodooHDA on Tahoe, selected automatically
* Disabled boot chime

### Setup

Before using this EFI, replace the following *PlatformInfo > Generic* values in *config.plist* with your own *use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)): `MLB`, `ROM`, `SystemSerialNumber` and `SystemUUID`.

Then apply the UEFI settings from [the UEFI configuration document](EXTRAS/HP_EliteDesk_800_G4_Mini_UEFI_Configuration.md).
Intel ME must be enabled for proper sleep/wake of the UHD 630.

### Audio on macOS Tahoe

Apple removed `AppleHDA` in macOS Tahoe, so **AppleALC.kext** no longer produces audio there. This EFI ships **VoodooHDA.kext** alongside it as a replacement, and **no configuration is needed** - the two are scoped by macOS version and never both drive the codec:

| Kext              | `MinKernel` | `MaxKernel` | Active on             |
| ----------------- | ----------- | ----------- | --------------------- |
| **AppleALC.kext** | `12.0.0`    | -           | Catalina … Sequoia    |
| **VoodooHDA.kext**| `25.0.0`    | -           | Tahoe and newer       |

`MinKernel` `25.0.0` is what keeps them apart: VoodooHDA attaches directly to the HDA PCI device, so on Sequoia and older it would compete with `AppleHDAController` - the version bound stops it from ever being injected there.
AppleALC needs no `MaxKernel` in return, because it works by patching `AppleHDA`; with `AppleHDA` gone in Tahoe it simply does nothing and never touches the device VoodooHDA claims. Leaving it loaded also means audio comes straight back if you ever restore `AppleHDA` on Tahoe via OCLP - in that case, disable **VoodooHDA.kext**, since `AppleHDAController` and VoodooHDA would then both want the same PCI device.

Two optional extras:

* **Volume and input gain control** - install [VoodooHDA.prefPane](https://github.com/CloverHackyColor/VoodooHDA/releases) to `/Library/PreferencePanes/`. Without it you get working audio but no fine-grained mixer control.
* **Layout matching** - VoodooHDA auto-detects the Conexant CX20632 by default, which is the tested   configuration. If you would rather have it follow the same AppleALC layout this EFI uses, add   `voodoo-layout-id` = `<14000000>` to *DeviceProperties > Add > PciRoot(0x0)/Pci(0x1F,0x3)*.

VoodooHDA is GPLv2 and is **not** an Acidanthera project - sources are at
[CloverHackyColor/VoodooHDA](https://github.com/CloverHackyColor/VoodooHDA).

### SIP and OpenCore Legacy Patcher

This EFI ships with SIP fully enabled (*NVRAM > Add > 7C436110-… > csr-active-config* = `<00000000>`) and does **not** bundle [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher). OCLP cannot be shipped inside an EFI folder - it is a macOS application that builds its own OpenCore EFI and root-patches the installed system volume, which breaks the APFS snapshot seal and requires AMFI and parts of SIP to be disabled.
The guide author [recommends against it](https://www.insanelymac.com/forum/topic/343937-guide-catalina-big-sur-monterey-ventura-sonoma-sequoia-tahoe-on-hp-elitedesk-800-g4g5-mini-the-perfect-macmini81-hackintosh/) on production machines for that reason, and the VoodooHDA route above avoids needing it for audio.

If you do need a different SIP level, change `csr-active-config`:

| Value        | Meaning                                                      |
| ------------ | ------------------------------------------------------------ |
| `<00000000>` | SIP fully enabled (default in this EFI)                       |
| `<01000000>` | Allow untrusted kexts - needed only to install VoodooHDA into `/Library/Extensions` instead of injecting it |
| `<03080000>` | Required by OCLP root patches                                 |

Injecting VoodooHDA via OpenCore as described above keeps SIP fully enabled, so `<00000000>` is fine.

### Images
![alt text](EXTRAS/images/system.png)

![alt text](EXTRAS/images/bootpicker.png)
