# OpencCore-HP-840-G1-Haswell

This HP Elitebook 840 G1 EFI configuration is based on, [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/haswell.html), and [RehabMan's HP guide](https://www.tonymacx86.com/threads/guide-hp-probook-elitebook-zbook-using-clover-uefi-hotpatch.261719/). This is also really useful.


**Contents**:
- [What is OpenCore?](#What-is-OpenCore?)
- [Hardware](#Hardware)
- [Changes](#Changes)
- [BIOS Settings](#bios-settings)
- [What Works](#What-Works)
- [Notes](#Notes)
- [Credit](#Credit)


## What is OpenCore?

OpenCore is an alternative bootloader to CloverEFI or Chameleon. It is not only for Hackintosh and can be used on real macs for purposes that require an emulated EFI. It also aims to have the ability to boot Windows and Linux. It has a clean codebase and aims to stay closer to how a real mac bootloader functions. Kext injection has been greatly improved.



## Hardware:

- **CPU**: Intel(R) i5-4300U (Haswell)
- **SSD**: Samsung SSD 860 EVO 500GB 
- **RAM**: 8GB DDR3
- **DISPLAY**: 1600x900 (Touchscreen)
- **GPU**: Integrated, Intel HD Graphics 4400
- **AUDIO CODEC**: IDT92HD91BXX
- **ETHERNET**: Intel I218-LM
- ~~**WIFI**: Intel Dual Band Wireless-N 7260NB 802.11 a/b/g/n (2 x2 ) WiFi  //  *It's not compatible.*~~
- **WIFI**: Intel Wi-Fi Drivers are NOW compatible for macOS, check [itlwm.](https://github.com/OpenIntelWireless/itlwm)

Here you can check the different models: https://support.hp.com/lv-en/document/c03961746

## Changes:

With Rehabman's SSDT-patches and kexts. You should boot OpenCore normally, but i have made very important changes to the ACPI-files and config.plist to make it as close to "Apple Like" as possible to fit the OpenCore mood.

* Removed ~~SSDT-HDEF and SSDT-HDAU audio patches from SSDT-8x0G1h.aml~~, and Applies AppleALC audio injection, you'll need to do your own research on which codec your motherboard has and match it with AppleALC's layout. [AppleALC Supported Codecs](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs).
* Replaced ~~IGPU.aml~~ With PNLF.aml for backlight controls to work properly with OpenCore.
* Removed ~~unnecessary config.plist patches that are made for other motherboards, like HP-840-G3~~.
* Removed ~~SSDT-USB-8x0-G1 and USBInjectAll.kext~~. Reason for this is USBInjectAll.kext is no longer being maintained and the USBmap.kext version is how real macs USB map. [USBMap](https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html).


## BIOS Settings:
###### *Spam the F10 key*


   
   - Disable
      - Fast Boot
      - Secure Boot
      - Serial/COM Port
      - Disable wake on USB
      - Parallel Port
      - VT-d (can be enabled if you set DisableIoMapper to YES)
      - CSM
      - Thunderbolt(For initial install, as Thunderbolt can cause issues if not setup correctly)
      - Intel SGX
      - Intel Platform Trust
      - CFG Lock (This must be off, otherwise your hack will not boot with CFG-Lock enabled) if you can't find the option then enable AppleXcpmCfgLock under: 
       
      ```
            Kernel -> Quirks -> AppleXcpmCfgLock=True
      ```

   - Enable
      - VT-x
      - Above 4G decoding
      - Hyper-Threading
      - Execute Disable Bit
      - EHCI/XHCI Hand-off
      - OS type: Windows 8.1/10 UEFI Mode
      - DVMT Pre-Allocated(iGPU Memory): 64MB
      - SATA Mode: AHCI

   - Note: Most of these options may not be present in your firmware, we recommend matching up as closely as possible but don't be too concerned if many of these options are not available in your BIOS.
   

Save and reboot in UEFI.


## What Works:

- **UEFI booting via OpenCore**
- **Dual Boot with Windows/Linux**
- **Built-in keyboard (with special function keys)**
- **Built-in trackpad (Multi-Gestures)**
- **Native WiFi & Bluetooth via Broadcom DW1560 BCM4352**
- **HDMI Audio/Video with hot-plug**
- **Native USB3 with AppleUSBXHCI using USBMap (USB2 works also)**
- **Native audio, including headphone**
- **Built-in mic**
- **Built-in camera**
- **Native power management**
- **Battery status**
- **Backlight controls with smooth transitions**
- **Accelerated graphics Intel HD Graphics 4400**
- **Wired Ethernet**
- **App Store**
- **iMessage & Facetime**

## Notes:

   - For Wifi and Bluetooth you have to replace your Wifi half mini PCI card to something that is supported by Apple. The HP-840-G1 doesn't have a Wifi whitelist, so in theory every card should work, but it has to be compatible with MacOS. I replaced it with a Broadcom DW1560 BCM4352 and worked Out of the Box, probably with the already installed kexts.

   - In order to use iMessage and Facetime you need to change your SMBIOS, with GenSMBIOS.
https://github.com/corpnewt/GenSMBIOS


## Credit:
   - [Apple](https://www.apple.com/) for MacOS
   - [vit9696](https://github.com/vit9696) for OpenCore
   - [Dortania's Guide](https://dortania.github.io/OpenCore-Install-Guide/) for OpenCore Install
   - [InsanelyMac's OpenCore forums](https://www.insanelymac.com/forum/topic/338516-opencore-discussion/) for finding issues with hardware and their work arounds
   - [RehabMan](https://github.com/RehabMan) for DSDT-Patches
   - [blint01](https://github.com/blint01/hackintosh-mojave-HP-840-G1) for Clover configuration guide
   - [itlwm](https://github.com/OpenIntelWireless/itlwm) Intel Wi-Fi Drivers for macOS
