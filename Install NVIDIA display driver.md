---
created: 2024-12-09T16:35:49Z
modified: 2024-12-09T16:35:49Z
---

## # Install NVIDIA display driver

```PowerShell
$setupPath = "\\TT-FS01\Products\Drivers\NVIDIA\GTX-1070" `
    + "\441.41-desktop-win10-64bit-international-whql.exe"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.
