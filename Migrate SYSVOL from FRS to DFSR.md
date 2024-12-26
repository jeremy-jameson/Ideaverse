---
created: 2024-12-24T10:01:40Z
modified: 2024-12-26T17:02:41Z
---

Reference:

[Streamlined Migration of FRS to DFSR SYSVOL | Microsoft Community Hub](https://techcommunity.microsoft.com/blog/filecab/streamlined-migration-of-frs-to-dfsr-sysvol/425405)

## Ensure free disk space

```PowerShell
function Format-SizeInBytes {
    param(
        [Parameter(Mandatory = $true, ValueFromPipeline = $true)]
        [uint64] $SizeInBytes
    )

    [float] $modifiedSize = $SizeInBytes

    [string[]] $units = "B", "KB", "MB", "GB", "TB", "PB"
    [int] $index = 0

    while ($modifiedSize -ge 1024 -and $index -lt ($units.Count - 1)) {
        $modifiedSize /= 1024
        $index++
    }

    [string] $formattedNumber = [string]::Empty

    if ($SizeInBytes -lt 1KB) {
        $formattedNumber = "{0:n0}" -f $modifiedSize
    }
    else {
        $formattedNumber = "{0:n2}" -f $modifiedSize
    }

    return ($formattedNumber + " " + $units[$index])
}

Get-WmiObject -Class Win32_LogicalDisk -ComputerName FAB-DC05, FAB-DC06 |
    sort SystemName, DeviceId |
    Format-Table SystemName, DeviceId,
        @{Name="FreeSpace";
            Expression={Format-SizeInBytes($_.FreeSpace)};
            Align="right"} `
        -AutoSize
```

Output:

| SystemName | DeviceId | FreeSpace |
| ---------- | -------- | --------- |
| …        | …      | …       |
| FAB-DC05   | D:       | 4.24 GB   |
| …        | …      | …       |
| FAB-DC06   | D:       | 4.26 GB   |

## Ensure correct security policy

```PowerShell
gpresult /s corp.fabrikam.com /h C:\NotBackedUp\Temp\gpresult.html
```

Output:

```text
ERROR: Invalid pointer
```

I spent several minutes googling this error, found several others reporting the same issues, but no solutions. Punt. 🏈

## Ensure AD replication is working

```PowerShell
repadmin /replsummary
```

Output:

```text
Replication Summary Start Time: 2024-12-24 03:51:46

Beginning data collection for replication summary, this may take awhile:
  …..


Source DSA          largest delta    fails/total %%   error
 FAB-DC05                  01m:27s    0 /   5    0
 FAB-DC06                  54m:57s    0 /   5    0


Destination DSA     largest delta    fails/total %%   error
 FAB-DC05                  54m:57s    0 /   5    0
 FAB-DC06                  01m:10s    0 /   5    0
```

## Ensure SYSVOL is shared

```PowerShell
dcdiag /e /test:sysvolcheck /test:advertising
```

Output:

```text
Directory Server Diagnosis

Performing initial setup:
   Trying to find home server…
   Home Server = FAB-DC05
   * Identified AD Forest.
   Done gathering initial info.

Doing initial required tests

   Testing server: Fabrikam-HQ\FAB-DC05
      Starting test: Connectivity
         ……………………. FAB-DC05 passed test Connectivity

   Testing server: Fabrikam-HQ\FAB-DC06
      Starting test: Connectivity
         ……………………. FAB-DC06 passed test Connectivity

Doing primary tests

   Testing server: Fabrikam-HQ\FAB-DC05
      Starting test: Advertising
         ……………………. FAB-DC05 passed test Advertising
      Starting test: SysVolCheck
         ……………………. FAB-DC05 passed test SysVolCheck

   Testing server: Fabrikam-HQ\FAB-DC06
      Starting test: Advertising
         Warning: FAB-DC06 is not advertising as a time server.
         ……………………. FAB-DC06 failed test Advertising
      Starting test: SysVolCheck
         ……………………. FAB-DC06 passed test SysVolCheck



   Running partition tests on : ForestDnsZones

   Running partition tests on : DomainDnsZones

   Running partition tests on : Schema

   Running partition tests on : Configuration

   Running partition tests on : corp

   Running enterprise tests on : corp.fabrikam.com
```

## Migrate to Prepared State

```PowerShell
dfsrmig /setglobalstate 1
```

```PowerShell
dfsrmig /getmigrationstate
```

## Migrate to Redirected State

```PowerShell
dfsrmig /setglobalstate 2
```

```PowerShell
dfsrmig /getmigrationstate
```

## Migrate to Eliminated State

```PowerShell
dfsrmig /setglobalstate 3
```

```PowerShell
dfsrmig /getmigrationstate
```
