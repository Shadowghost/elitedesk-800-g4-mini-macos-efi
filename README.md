## HP EliteDesk 800 G4 Mini OpenCore EFI

[![latest release](https://img.shields.io/github/v/release/Shadowghost/elitedesk-800-g4-mini-macos-efi?label=latest%20release)](https://github.com/Shadowghost/elitedesk-800-g4-mini-macos-efi/releases)

This repository contains a generic [OpenCore](https://github.com/acidanthera/OpenCorePkg) based **EFI folder** for booting macOS on **HP EliteDesk 800 G4 Mini** computers.

This EFI is based off the excellent work of [deeveedee](https://www.insanelymac.com/forum/profile/1099364-deeveedee/) over at [insanelymac](https://www.insanelymac.com/forum/topic/343937-guide-catalina-big-sur-monterey-ventura-on-hp-elitedesk-800-g4g5-mini-the-perfect-macmini81-hackintosh/).

For more information on how to install macOS on a **HP EliteDesk 800 G4 Mini** please consult the linked thread above.

For convenience sake this repository includes the recommended UEFI configuration and additional *DeviceProperties* configurations in the [EXTRAS folder](EXTRAS).

Features of this EFI:

* Working macOS boot
* Working BootCamp
* Graphical boot picker via OpenCanopy with [BigSurFlat theme](https://github.com/82ghost82/BigSurFlat)
* 15 port limit of BigSur is complied with by disabling the internal ports for the WiFi/BT module
* Disabled boot chime

### Images
![alt text](EXTRAS/images/system.png)

![alt text](EXTRAS/images/bootpicker.png)
