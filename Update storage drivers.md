---
created: 2025-01-13T16:15:52Z
modified: 2025-01-17T14:36:58Z
---

#### Update storage drivers

1. Copy the C600 chipset driver files to a temporary location on the server.
1. Install the drivers for the **Intel(R) C600+/C220+ series chipset SATA AHCI Controller (PCI\\VEN_8086&DEV_8D02&…)**.
1. Restart the server.

```PowerShell
$source = "\\TT-FS01\Public\Download\Drivers\Intel" `
    + "\RSTe AHCI & SCU Software RAID driver for Windows\F6-drivers" `
    + "\RSTe_4.7.0.1119_F6-drivers\iaStorA.free.win8.64bit.4.7.0.1098"

$destination = "C:\NotBackedUp\Temp\Drivers\Intel\" `
    + "iaStorA.free.win8.64bit.4.7.0.1098"

robocopy $source $destination /E

pushd $destination

pnputil /add-driver *.inf /install /reboot
```
