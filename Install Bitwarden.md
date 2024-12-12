---
created: 2024-12-09T17:07:36Z
modified: 2024-12-09T17:07:36Z
---

### # Install Bitwarden

```PowerShell
net use \\TT-FS01\Products /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Bitwarden\Bitwarden-Installer-1.16.6.exe"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.
