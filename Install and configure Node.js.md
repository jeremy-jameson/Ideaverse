
```PowerShell
cls
```

## # Install and configure Node.js

### # Install Node.js

```PowerShell
net use \\EXT-FS01\IPC$ /USER:EXTRANET\jjameson-admin
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\node.js\node-v12.13.1-x64.msi"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete. Restart PowerShell for the change to PATH environment variable to take effect.

```Console
exit
```

### # Change NPM file locations to avoid issues with redirected folders

```PowerShell
notepad "C:\Program Files\nodejs\node_modules\npm\npmrc"
```

---

File - **C:\\Program Files\\nodejs\\node_modules\\npm\\npmrc**

```Text
;prefix=${APPDATA}\npm
prefix=${LOCALAPPDATA}\npm
cache=${LOCALAPPDATA}\npm-cache
```

---
