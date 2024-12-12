---
created: 2024-12-09T17:36:25Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

## # Configure development environment

### # Configure NuGet global package location

```PowerShell
[Environment]::SetEnvironmentVariable(
    "NUGET_PACKAGES",
    "C:\NotBackedUp\.nuget\packages",
    "Machine")
```

### # Configure symbol path for debugging

```PowerShell
$symbolPath = "SRV*C:\NotBackedUp\Public\Symbols" `
     + "*\\TT-FS01\Public\Symbols" `
     + "*https://msdl.microsoft.com/download/symbols"

[Environment]::SetEnvironmentVariable(
    "_NT_SYMBOL_PATH",
    $symbolPath,
    "Machine")
```

```PowerShell
cls
```

### # Install reference assemblies

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$source = '\\TT-FS01\Builds\Reference Assemblies'
$destination = 'C:\Program Files\Reference Assemblies'

robocopy $source $destination /E

& "$destination\Microsoft\SharePoint v4\AssemblyFoldersEx - x64.reg"
```

![(screenshot)](https://assets.technologytoolbox.com/screenshots/62/DE96621F16BC51A75B6410BE1B94F33F9B8A0F62.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5E/06DBAE68F16F7F83442A5F1916BFBEFEFC0DD15E.png)

```PowerShell
& "$destination\Microsoft\SharePoint v5\AssemblyFoldersEx - x64.reg"
```

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AE/E9FD2601E62121DA76E1227B4436C7ED8F069CAE.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BB/9A7DF5FF756E44DAC0D8F600AA2D457598370EBB.png)

```PowerShell
cls
```
