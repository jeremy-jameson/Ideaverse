---
created: 2024-12-09T17:16:00Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

## # Install Visual Studio 2019

### # Launch Visual Studio 2019 setup

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Visual Studio 2019\Enterprise" `
    + "\vs_setup.exe"

Start-Process -FilePath $setupPath -Wait
```

Select the following workloads:

- **ASP.NET and web development**
- **Azure development**
- **Python development**
- **Node.js development**
- **.NET desktop development**
- **Desktop development with C++**
- **Universal Windows Platform development**
- **Data storage and processing**
- **Data science and analytical applications**
- **Office/SharePoint development**
- **.NET Core cross-platform development**

> **Note**
>
> When prompted, restart the computer to complete the installation.
