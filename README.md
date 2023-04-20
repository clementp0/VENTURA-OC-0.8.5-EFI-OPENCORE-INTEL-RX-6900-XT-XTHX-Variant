
  

# EFI-OPENCORE-I9-10900KF-RX-6900-XT-XTHX-Variant

  

OPENCORE 0.8.5 EFI using I9 10900KF and RX 6900 XT XTHX Variant (0x73af)

  

![nfo](https://cdn.discordapp.com/attachments/489413377676804147/1089880320540672120/I9.png)

  

**Latest working macOS**: Ventura 13.0

  

**Current OpenCore**: 0.8.5

  

RX 6900 XT :

```

The XTXH variant (Device ID: 0x73AF) is supported with WhateverGreen 1.5.2 and spoofing device-id to 0x73BF.

```

See [documentation](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html#navi-21-series)

  
  

## Complete hardware specs

  

- Intel i9 10900k

- ROG STRIX Z490-E GAMING

- AMD Radeon AORUS RX 6900 XT MASTER 16G (0x73af)

- 32GB RAM Corsair Dominator Platinum - 3400 MHz DDR4

  

## What works

  

- macOS Monterey 12.6, macOS Ventura 13.0

- WiFi (using [itlwm](https://github.com/OpenIntelWireless/itlwm) + [heliport](https://github.com/OpenIntelWireless/HeliPort))

- Audio

- HDMI/DP

- All USB ports

- 2x 2.5Gbit Ethernet

- Everything iCloud related (Drive, iMessage, Facetime, etc)

- Temperature monitoring for everything except GPU (no GPU temp support in VirtualSMC for navi and big navi cards)

- DRM content (Netflix, ATV+, Airplay 2 mirroring etc)

- Shutdown/Reboot

- Deep Sleep

  

## What doesn't work

  

- (oob) WiFi and Bluetooth + Airdrop + Sidecar..

*(will be fixed with BCM94360CD card)*

  

## Kexts used:

  

- Lilu.kext

- itlwm.kext

- Lilu.kext

- USBInjectAll.kext

- VirtualSMC.kext

- WhateverGreen.kext

-  *Other Kext needed for my config (and residuals)*

  

## SSDT used:

  

- SSDT-BRG0.aml

- SSDT-GPU-SPOOF.aml

  

## How to use it ?

  

⚠️ Dont forget to recreate the _plateforminfo_ section and change SMBIOS data (using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) or any other tool) ⚠️

  

Open /EFI/OC/ACPI/**SSDT-GPU-SPOOF.aml** with [MaciASL](https://github.com/acidanthera/MaciASL).

  

Edit this part :

  
  

..........................................................

    DefinitionBlock ("", "SSDT", 2, "DRTNIA", "AMDGPU", 0x00001000)
    
    {
    
    External (_SB_.PCI0, DeviceObj)
    
    External (_SB_.PCI0.PEG0.PEGP, DeviceObj)
    
    Scope (\_SB.PCI0.PEG0.PEGP)
    
    {
    
    If (_OSI ("Darwin"))
    
    {
    
    Method (_DSM, 4, NotSerialized) // _DSM: Device-Specific Method
    
    {
    
    Local0 = Package (0x04)
    
    {
    
    "device-id",
    
    Buffer (0x07)
    
    {
    
    "0x73BF"
    
    },
    
    "model",
    
    Buffer (0x1D)
    
    {
    
    "AMD Radeon RX 6900 XT"
    
    }
    
    }
    
    DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
    
    Return (Local0)
    
    }
    
    }
    
    }

..........................................................

  

**Edit :**

`External (_SB_.PCI0, DeviceObj)` & `External (_SB_.PCI0.PEG0.PEGP, DeviceObj)`

  

**For example :**

```

ACPI(_SB_)#ACPI(PC02)#ACPI(BR2A)#ACPI(PEGP)#PCI(0000)#PCI(0000)

```

  

Now converting this to an ACPI path is quite simple, remove the `#ACPI` and `#PCI(0000)`:

  

```

\_SB_.PC02.BR2A.PEGP

```

  

*Dont forget to compile it back.*

  
  

See [documentation](https://dortania.github.io/Getting-Started-With-ACPI/Universal/spoof.html)

## Thanks/Credits

  

- [Opencore Team](https://dortania.github.io/getting-started/)

- [luchina-gabriel](https://github.com/luchina-gabriel) for README shape
