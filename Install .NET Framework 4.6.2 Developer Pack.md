---
created: 2024-12-09T17:18:19Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

### # Install .NET Framework 4.6.2 Developer Pack

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Public\Download\Microsoft\dotNet" `
    + "\NDP462-DevPack-KB3151934-ENU.exe"

Start-Process -FilePath $setupPath -Wait
```
