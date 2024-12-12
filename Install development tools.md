---
created: 2024-12-09T17:35:01Z
modified: 2024-12-12T11:45:42Z
---

## # Install development tools

### # Install IIS

```PowerShell
Enable-WindowsOptionalFeature `
    -Online `
    -All `
    -FeatureName IIS-ManagementConsole, `
        IIS-ASPNET45, `
        IIS-DefaultDocument, `
        IIS-DirectoryBrowsing, `
        IIS-HttpErrors, `
        IIS-StaticContent, `
        IIS-HttpLogging, `
        IIS-HttpCompressionStatic, `
        IIS-RequestFiltering, `
        IIS-WindowsAuthentication
```

```PowerShell
cls
```

### # Install SharePoint Online Management Shell

```PowerShell
$installerPath = "\\TT-FS01\Public\Download\Microsoft\SharePoint\Online" `
    + "\SharePointOnlineManagementShell_19418-12000_x64_en-us.msi"

Start-Process `
    -FilePath msiexec.exe `
    -ArgumentList "/i `"$installerPath`"" `
    -Wait
```

> **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

### # Install SharePoint PnP cmdlets

```PowerShell
Install-Module SharePointPnPPowerShellOnline
```

```PowerShell
cls
```

### # Install Chocolatey

```PowerShell
((New-Object System.Net.WebClient).DownloadString(
    'https://chocolatey.org/install.ps1')) |
    Invoke-Expression
```

> **Important**
>
> Restart PowerShell for environment changes to take effect.

```Console
exit
```

### Reference

**Installing Chocolatey**\
From <[https://chocolatey.org/install](https://chocolatey.org/install)>

### # Install HTML Tidy

```PowerShell
choco install html-tidy
```

The recent package changes indicate a reboot is necessary.\
Please reboot at your earliest convenience.

```PowerShell
Restart-Computer
```

### # Install Minikube

```PowerShell
choco install minikube
```

```PowerShell
cls
```

#### # Start Minikube

```PowerShell
minikube start --vm-driver=hyperv
```

#### Reference

**Install Minikube**\
From <[https://kubernetes.io/docs/tasks/tools/install-minikube/](https://kubernetes.io/docs/tasks/tools/install-minikube/)>

```PowerShell
cls
```

### # Install FileZilla

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath =
"\\TT-FS01\Products\FileZilla\FileZilla_3.46.0_win64-setup.exe"

Start-Process -FilePath $setupPath -Wait
```

```PowerShell
cls
```

### # Install Postman

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Postman\Postman-win64-7.13.0-Setup.exe"

Start-Process -FilePath $setupPath -Wait
```

```PowerShell
cls
```

### # Install Fiddler

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Telerik\FiddlerSetup.exe"

$arguments = "/S /D=C:\Program Files\Telerik\Fiddler"

Start-Process `
    -FilePath $setupPath `
    -ArgumentList $arguments `
    -Wait
```

### Reference

**Default Install Path**\
[https://www.telerik.com/forums/default-install-path#t8qFEfoMnUWlg50zptFlHQ](https://www.telerik.com/forums/default-install-path#t8qFEfoMnUWlg50zptFlHQ)

```PowerShell
cls
```

### # Install Microsoft Message Analyzer

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Message Analyzer 1.4\MessageAnalyzer64.msi"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.

"Windows blocked the installation of a digitally unsigned driver…"

**Microsoft Message Anlayzer 1.4 - 'A Digitally Signed Driver Is Required'**\
From <[https://social.technet.microsoft.com/Forums/windows/en-US/48b4c226-fc3d-4793-b544-3440ed13424a/microsoft-message-anlayzer-14-a-digitally-signed-driver-is-required?forum=messageanalyzer](https://social.technet.microsoft.com/Forums/windows/en-US/48b4c226-fc3d-4793-b544-3440ed13424a/microsoft-message-anlayzer-14-a-digitally-signed-driver-is-required?forum=messageanalyzer)>

```PowerShell
cls
```

### # Install Wireshark

```PowerShell
$setupPath = "\\TT-FS01\Products\Wireshark\Wireshark-win64-3.0.7.exe"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

### # Install Microsoft Log Parser 2.2

```PowerShell
$setupPath = "\\TT-FS01\Public\Download\Microsoft\LogParser 2.2\LogParser.msi"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

### # Install Remote Desktop Connection Manager

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft" `
    + "\Remote Desktop Connection Manager\rdcman.msi"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

### # Install Microsoft Expression Studio 4

### # Copy installation media from internal file server

```PowerShell
$isoFile = "en_expression_studio_4_ultimate_x86_dvd_537032.iso"

$source = "\\TT-FS01\Products\Microsoft\Expression Studio"

$destination = "C:\NotBackedUp\Temp"

robocopy $source $destination $isoFile
```

### Install Expression Studio

Use the "Toolbox" script to install Expression Studio

```PowerShell
cls
```

## # Install Paint.NET

```PowerShell
& "\\TT-FS01\Products\Paint.NET\paint.net.4.2.8.install.zip"
```

> **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```
