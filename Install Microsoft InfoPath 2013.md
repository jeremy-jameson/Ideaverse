```PowerShell
cls
```

### # Install Microsoft InfoPath 2013

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Office 2013" `
    + "\infopath_4753-1001_x86_en-us.exe"

Start-Process `
    -FilePath $setupPath `
    -Wait
```

> **Important**
>
> Wait for the installation to complete.
