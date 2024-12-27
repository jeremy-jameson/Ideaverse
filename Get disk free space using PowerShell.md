---
created: 2024-12-23T16:42:23Z
modified: 2024-12-27T20:13:11Z
---

After once again running out of disk space while patching Windows, I decided to spend a few minutes researching how to use PowerShell to easily check the available free space across a number of computers.

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
```

## CONTOSO domain

```PowerShell
$computersToExclude = @(
    'CON-UBU18-DEV1'  # Ubuntu server
    )

$computers = Get-ADComputer -Filter * |
    where { $_.Name -notin $computersToExclude } |
    select -ExpandProperty Name

Get-WmiObject -Class Win32_LogicalDisk -ComputerName $computers |
    where { $_.DeviceId -notin @('A:', 'X:') } |
    sort SystemName, DeviceId |
    select SystemName, DeviceId,
        @{Name="FreeSpace";Expression={Format-SizeInBytes($_.FreeSpace)}} |
    Format-Table -AutoSize
```

### Output

| SystemName      | DeviceId | FreeSpace |
|-----------------|----------|-----------|
| CON-ADFS01      | C:       |           |
| CON-DC07        | C:       |           |
| CON-DC07        | D:       |           |
| CON-DC08        | C:       |           |
| CON-DC08        | D:       |           |
| CON-W10-TEST-03 | C:       |           |

## EXTRANET domain

```PowerShell
$computersToExclude = @(
    'EXT-ADFS01A',
    'EXT-ADFS03A',
    'EXT-APP03A',
    'EXT-FOOBAR2',
    'EXT-SQL01',       # SQL Server failover cluster
    'EXT-SQL01-FC'     # SQL Server failover cluster
    'EXT-SEQ02',       # Ubuntu server
    'EXT-UD20-DEV-01', # Ubuntu server
    'EXT-VS2010-DEV1',
    'EXT-VS2013-DEV1',
    'EXT-WAC02A',
    'EXT-WAP01A',
    'EXT-WAP03A',
    'EXT-WEB03A'
    )

$computers = Get-ADComputer -Filter * |
    where { $_.Name -notin $computersToExclude } |
    select -ExpandProperty Name

Get-WmiObject -Class Win32_LogicalDisk -ComputerName $computers |
    where { $_.DeviceId -notin @('A:', 'X:') } |
    sort SystemName, DeviceId |
    select SystemName, DeviceId,
        @{Name="FreeSpace";Expression={Format-SizeInBytes($_.FreeSpace)}} |
    Format-Table -AutoSize
```

### Output

| SystemName  | DeviceId | FreeSpace |
| ----------- | -------- | --------: |
| EXT-ADFS03B | C:       |  12.08 GB |
| EXT-DC10    | C:       |   9.78 GB |
| EXT-DC10    | D:       |   4.29 GB |
| EXT-DC11    | C:       |   9.14 GB |
| EXT-DC11    | D:       |   4.31 GB |
| EXT-FOOBAR8 | C:       |  18.29 GB |
| EXT-FOOBAR8 | D:       | 209.80 GB |
| …         | …      |       … |
| EXT-WEB03B  | C:       |   7.09 GB |
| EXT-WEB03B  | D:       |  38.84 GB |
| EXT-WEB03B  | E:       |    0.00 B |
| EXT-WEB03B  | L:       |  18.97 GB |

## FABRIKAM domain

```PowerShell
$computersToExclude = @('EXCHANGE-CAS', 'FAB-TEST1')

$computers = Get-ADComputer -Filter * |
    where { $_.Name -notin $computersToExclude } |
    select -ExpandProperty Name

Get-WmiObject -Class Win32_LogicalDisk -ComputerName $computers |
    where { $_.DeviceId -notin @('A:', 'X:') } |
    sort SystemName, DeviceId |
    select SystemName, DeviceId,
        @{Name="FreeSpace";Expression={Format-SizeInBytes($_.FreeSpace)}} |
    Format-Table -AutoSize
```

### Output

| SystemName  | DeviceId | FreeSpace |
| ----------- | -------- | --------: |
| FAB-ADFS02  | C:       |  12.46 GB |
| FAB-ADMIN01 | C:       |  16.80 GB |
| FAB-DC05    | C:       |  12.29 GB |
| FAB-DC05    | D:       |   4.24 GB |
| FAB-DC06    | C:       |  12.28 GB |
| FAB-DC06    | D:       |   4.27 GB |
| FAB-FS01    | C:       |  18.91 GB |
| FAB-FS01    | D:       |  49.10 GB |
| FAB-WEB01   | C:       |  14.58 GB |

## TECHTOOLBOX domain

```PowerShell
$computersToExclude = @(
    'TT-BUILD01',     # Offline (obsolete) build server for TFS
    'TT-DOCKER01',    # Ubuntu server
    'TT-DOCKER02',    # Ubuntu server
    'TT-HV06-FC',     # Hyper-V failover cluster
    'TT-MAIL-TEST01', # Ubuntu server
    'TT-MDT-DEV1', # Offline development VM for Microsoft Deployment Toolkit
    'TT-MDT-DEV2', # Offline development VM for Microsoft Deployment Toolkit
    'tt-nas02',       # TrueNAS server
    'TT-SQL01',       # SQL Server failover cluster
    'TT-SQL01-FC',    # SQL Server failover cluster
    'TT-SQL02',       # Offline (obsolete) SQL server for TFS
    'TT-SQL2017-DEV', # Offline (obsolete) SQL development VM
    'TT-TFS02',       # Offline (obsolete) Team Foundation Server
    'TT-TFS2018-DEV', # Offline (obsolete) TFS development VM
    'TT-UD18-DEV-01', # Ubuntu desktop
    'TT-UD18-DEV-02', # Ubuntu desktop
    'TT-UD18-DEV-03', # Ubuntu desktop
    'TT-UD18-DEV-04', # Ubuntu desktop
    'TT-UD20-DEV-01', # Ubuntu desktop
    'TT-VMM01',       # Virtual Machine Manager failover cluster
    'TT-VMM01-FC',    # Virtual Machine Manager failover cluster
    'TT-VS2013-DEV',  # Offline/development/test VMs
    'TT-W10-DEV09',
    'TT-W10-DEV10',
    'TT-W11-DEV-01',
    'TT-W11-DEV-02',
    'TT-W7-TEST-04',
    'TT-W7-TEST-05',
    'TT-W7-TEST-06',
    'TT-W8-TEST-02',
    'TT-WEB01-DEV',
    'TT-WEB02-DEV',
    'TT-WEB03-DEV',
    'TT-WIN10-DEV1',
    'TT-WIN10-DEV4',
    'TT-WIN10-DEV6',
    'TT-WIN8-TEST1'
    )

$computers = Get-ADComputer -Filter * |
    where { $_.Name -notin $computersToExclude } |
    select -ExpandProperty Name

Get-WmiObject -Class Win32_LogicalDisk -ComputerName $computers |
    where { $_.DeviceId -notin @('A:', 'X:') } |
    sort SystemName, DeviceId |
    select SystemName, DeviceId,
        @{Name="FreeSpace";Expression={Format-SizeInBytes($_.FreeSpace)}} |
    Format-Table -AutoSize
```

### Output

| SystemName | DeviceId | FreeSpace  |
|------------|----------|------------|
| CIPHER01   | C:       | 11.67 GB   |
| HAVOK-TEST | C:       | 2.31 GB    |
| HAVOK-TEST | D:       | 23.20 GB   |
| HAVOK-TEST | L:       | 7.30 GB    |
| HAVOK-TEST | T:       | 990.69 MB  |
| HAVOK-TEST | Z:       | 49.90 GB   |
| TT-ADMIN04 | C:       | 4.86 GB    |
| TT-ADMIN05 | C:       | 9.31 GB    |
| TT-DC12    | C:       | 9.30 GB    |
| TT-DC12    | D:       | 4.11 GB    |
| TT-DC12    | Z:       | 10.04 GB   |
| …        | …      | …        |
| TT-SQL03   | C:       | 10.15 GB   |
| TT-SQL03   | D:       | 7.50 GB    |
| TT-SQL03   | L:       | 2.14 GB    |
| TT-SQL03   | T:       | 1.42 GB    |
| TT-SQL03   | Z:       | 12.86 GB   |
| TT-VMM01C  | C:       | 10.48 GB   |
| TT-VMM01D  | C:       | 11.32 GB   |
| TT-WSUS04  | C:       | 9.72 GB    |
| TT-WSUS04  | D:       | 89.53 GB   |
