---
created: 2024-12-09T16:49:50Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

### # Configure storage

#### # Change drive letter for DVD-ROM

```PowerShell
$cdrom = Get-WmiObject -Class Win32_CDROMDrive
$driveLetter = $cdrom.Drive

$volumeId = mountvol $driveLetter /L
$volumeId = $volumeId.Trim()

mountvol $driveLetter /D

mountvol X: $volumeId
```

#### Physical disks

| Disk | Model | Serial Number | Capacity | Drive Letter | Volume Size | Allocation Unit Size | Volume Label |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | Samsung SSD 850 PRO 256GB | \*\*\*\*\*\*\*\*\*19550Z | 256 GB | C: | 235 GB | 4K | System |
| 1 | Samsung Samsung SSD 860 EVO 1TB | \*\*\*\*\*\*\*\*\*26709R | 1 TB | E: | 931 GB | 4K | Gold01 |
| 2 | WDC WD1001FALS-00E3A0 | WD-\*\*\*\*\*\*283566 | 1 TB | Z: | 931 GB | 4K | Backup01 |
| 3 | WDC WD1002FAEX-00Y9A0 | WD-\*\*\*\*\*\*201582 | 1 TB | F: | 931 GB | 4K | Bronze01 |
| 4 | ST1000NM0033-9ZM173 | \*\*\*\*\*EMV | 1 TB |  |  |  |  |
| 5 | ST1000NM0033-9ZM173 | \*\*\*\*\*4YL | 1 TB |  |  |  |  |
| 6 | Samsung SSD 970 PRO 512GB | \*\*\*\*\*\*\*\*\*\_81B1_6431. | 512 GB | D: |  | 4K | Platinum01 |

```PowerShell
Get-PhysicalDisk | sort DeviceId

Get-PhysicalDisk | select DeviceId, Model, SerialNumber, CanPool |
    sort DeviceId | Format-Table -AutoSize
```

```PowerShell
cls
```

#### # Configure partitions and volumes

##### # Rename "Windows" volume to "System"

```PowerShell
Get-Volume -DriveLetter C | Set-Volume -NewFileSystemLabel System
```
