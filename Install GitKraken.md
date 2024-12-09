```PowerShell
cls
```
## # Install GitKraken

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Axosoft\GitKrakenSetup-6.3.1.exe"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.
