---
tags: [017, 636]
created: 2024-12-19T12:23:46Z
modified: 2024-12-19T23:27:56Z
---

```Text
12345678901234567890123456789012345678901234567890123456789012345678901234567890
```

## Install Windows 11 Enterprise (x64)

### Create local administrator account and set password

## Install additional browsers and various software

### Install Firefox

### Install Google Chrome

```PowerShell
cls
```

### # Copy Toolbox content

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
robocopy \\TT-FS01\Public\Toolbox C:\NotBackedUp\Public\Toolbox /E /NP
```

```PowerShell
cls
```

## # Create default folders and configure custom icons

### # Create default folders

```PowerShell
mkdir C:\BackedUp
mkdir C:\NotBackedUp\Public\Symbols
mkdir C:\NotBackedUp\Temp
```

### Configure custom icons for folders

- C:\\BackedUp
- C:\\NotBackedUp
- C:\\NotBackedUp\\Public
- C:\NotBackedUp\Publc\Symbols
- C:\\NotBackedUp\\Public\\Toolbox
- C:\\NotBackedUp\\Temp

```PowerShell
cls
```

### # Set time zone

```PowerShell
tzutil /s "Mountain Standard Time"
```

```PowerShell
cls
```

## # Rename computer and join domain

```PowerShell
$computerName = "TT-W11-DEV-02"

Rename-Computer -NewName $computerName -Restart
```

Wait for the computer to restart and then execute the following command to join the **TECHTOOLBOX** domain:

```PowerShell
Add-Computer -DomainName corp.technologytoolbox.com -Restart
```

---

**TT-ADMIN05** - Run as administrator

```PowerShell
cls
```

### # Move computer to different OU

```PowerShell
$computerName = "TT-W11-DEV-02"

$targetPath = ("OU=Workstations,OU=Resources,OU=Development" `
    + ",DC=corp,DC=technologytoolbox,DC=com")

Get-ADComputer $computerName | Move-ADObject -TargetPath $targetPath
```

---

### Login as local administrator account

### # Configure networking

```PowerShell
$interfaceAlias = "LAN"
```

#### # Rename network connections

```PowerShell
Get-NetAdapter -Physical | select InterfaceDescription

$interfaceDescription = "Microsoft Hyper-V Network Adapter"

Get-NetAdapter -InterfaceDescription $interfaceDescription |
    Rename-NetAdapter -NewName $interfaceAlias
```

#### # Enable jumbo frames

```PowerShell
Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Set-NetAdapterAdvancedProperty `
    -Name $interfaceAlias `
    -DisplayName "Jumbo Packet" `
    -RegistryValue 9014

Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Start-Sleep -Seconds 5

ping TT-FS01 -f -l 8900
```

```PowerShell
cls
```

### # Disable 'Automatically detect proxy settings' in Internet Explorer

```PowerShell
Function Disable-AutomaticallyDetectProxySettings {
    # Read connection settings from Internet Explorer.
    $regKeyPath = "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\Connections\"
    $conSet = $(Get-ItemProperty $regKeyPath).DefaultConnectionSettings

    # Index into DefaultConnectionSettings where the relevant flag resides.
    $flagIndex = 8

    # Bit inside the relevant flag which indicates whether or not to enable automatically detect proxy settings.
    $autoProxyFlag = 8

    if ($($conSet[$flagIndex] -band $autoProxyFlag) -eq $autoProxyFlag)
    {
        # 'Automatically detect proxy settings' was enabled, adding one disables it.
        Write-Host "Disabling 'Automatically detect proxy settings'."
        $mask = -bnot $autoProxyFlag
        $conSet[$flagIndex] = $conSet[$flagIndex] -band $mask
        $conSet[4]++
        Set-ItemProperty -Path $regKeyPath -Name DefaultConnectionSettings -Value $conSet
    }

    $conSet = $(Get-ItemProperty $regKeyPath).DefaultConnectionSettings
    if ($($conSet[$flagIndex] -band $autoProxyFlag) -ne $autoProxyFlag)
    {
        Write-Host "'Automatically detect proxy settings' is disabled."
    }
}

Disable-AutomaticallyDetectProxySettings
```

#### Reference

**Disable-AutomaticallyDetectSettings.ps1**\
From <[https://gist.github.com/ReubenBond/1387620](https://gist.github.com/ReubenBond/1387620)>

```PowerShell
cls
```

### # Configure storage

#### # Configure partitions and volumes

##### # Rename "Windows" volume to "System"

```PowerShell
Get-Volume -DriveLetter C | Set-Volume -NewFileSystemLabel System
```

```PowerShell
cls
```

### # Enable PowerShell remoting

```PowerShell
Enable-PSRemoting -Confirm:$false
```

### # Configure firewall rules for [http://poshpaig.codeplex.com/](POSHPAIG)

```PowerShell
Set-ExecutionPolicy RemoteSigned -Scope Process -Force

C:\NotBackedUp\Public\Toolbox\PowerShell\Enable-RemoteWindowsUpdate.ps1 -Verbose
C:\NotBackedUp\Public\Toolbox\PowerShell\Disable-RemoteWindowsUpdate.ps1 `
    -Verbose
```

```PowerShell
cls
```

## # Configure backup

### # Install DPM agent

```PowerShell
$installerPath = "\\TT-FS01\Products\Microsoft\System Center 2019" `
    + "\DPM\Agents\DPMAgentInstaller_x64.exe"

$installerArguments = "TT-DPM06.corp.technologytoolbox.com"

Start-Process `
    -FilePath $installerPath `
    -ArgumentList "$installerArguments" `
    -Wait
```

Review the licensing agreement. If you accept the Microsoft Software License Terms, select **I accept the license terms and conditions**, and then click **OK**.

Confirm the agent installation completed successfully and the following firewall exceptions have been added:

- Exception for DPMRA.exe in all profiles
- Exception for Windows Management Instrumentation service
- Exception for RemoteAdmin service
- Exception for DCOM communication on port 135 (TCP and UDP) in all profiles

#### Reference

**Installing Protection Agents Manually**\
Pasted from <[http://technet.microsoft.com/en-us/library/hh757789.aspx](http://technet.microsoft.com/en-us/library/hh757789.aspx)>

---

**TT-ADMIN05** - DPM Management Shell

```PowerShell
cls
```

### # Attach DPM agent

```PowerShell
$productionServer = 'TT-W11-DEV-02'

.\Attach-ProductionServer.ps1 `
    -DPMServerName TT-DPM06 `
    -PSName $productionServer `
    -Domain TECHTOOLBOX `
    -UserName jjameson-admin
```

---

### Add virtual machine to DPM protection group

```PowerShell
cls
```

### # Configure antivirus on DPM protected server

#### # Disable real-time monitoring by Windows Defender for DPM server

```PowerShell
[array] $excludeProcesses = Get-MpPreference |
    select -ExpandProperty ExclusionProcess

$excludeProcesses +=
   "$env:ProgramFiles\Microsoft Data Protection Manager\DPM\bin\DPMRA.exe"

Set-MpPreference -ExclusionProcess $excludeProcesses
```

#### # Configure antivirus software to delete infected files

```PowerShell
Set-MpPreference -LowThreatDefaultAction Remove
Set-MpPreference -ModerateThreatDefaultAction Remove
Set-MpPreference -HighThreatDefaultAction Remove
Set-MpPreference -SevereThreatDefaultAction Remove
```

#### Reference

**Run antivirus software on the DPM server**\
From <[https://docs.microsoft.com/en-us/system-center/dpm/run-antivirus-server?view=sc-dpm-2019](https://docs.microsoft.com/en-us/system-center/dpm/run-antivirus-server?view=sc-dpm-2019)>

TODO:

```PowerShell
cls
```

## # Install Visual Studio Code

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Visual Studio Code" `
    + "\VSCodeSetup-x64-1.95.3.exe"

$arguments = "/silent" `
    + " /mergetasks='!runcode,addcontextmenufiles,addcontextmenufolders" `
        + ",associatewithfiles,addtopath'"

Start-Process `
    -FilePath $setupPath `
    -ArgumentList $arguments `
    -Wait
```

> [!important] **Important**
>
> Wait for the installation to complete.

### Issue

**Installer doesn't disable launch of VScode even when installing with
/mergetasks=!runcode**\
From <[https://github.com/Microsoft/vscode/issues/46350](https://github.com/Microsoft/vscode/issues/46350)>

### Set environment variable to override location of Visual Studio Code extensions

```PowerShell
[Environment]::SetEnvironmentVariable(
    "VSCODE_EXTENSIONS",
    "C:\NotBackedUp\Public\vscode-extensions",
    "Machine")
```

#### Reference

[Have a setting for the extensions folder location · Issue #17691 · microsoft/vscode](https://github.com/microsoft/vscode/issues/17691)
---

```PowerShell
cls
```

### # Copy Visual Studio Code extensions from file share

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
robocopy \\TT-FS01\Public\vscode-extensions `
    C:\NotBackedUp\Public\vscode-extensions /E /NP
```

### TODO: Configure Visual Studio Code settings

1. Press **Ctrl+Shift+P**
1. Select **Preferences: Open Settings (JSON)**

---

File - **settings.json**

```JSON
{
    "editor.formatOnSave": true,
    "editor.renderWhitespace": "boundary",
    "editor.rulers": [80],
    "files.trimTrailingWhitespace": true,
    "git.autofetch": true,
    "html.format.wrapLineLength": 80,
    "prettier.disableLanguages": ["html"],
    "terminal.integrated.shell.windows":
        "C:\\windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
    "workbench.iconTheme": "vscode-icons"
}
```

---

```PowerShell
cls
```

## # Install Obsidian

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Obsidian\Obsidian-1.7.7.exe"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
>
> On the **Choose Installation Options** step, select **Anyone who uses this computer (all users)**.

```PowerShell
cls
```

## # Install and configure Git

### # Install Git

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Git\Git-2.47.1-64-bit.exe"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
>
> 1. On the **Choosing the default editor used by Git** step, select **Use Visual Studio Code as Git's default editor**.
> 1. On the **Adjusting the name of the initial branch in new repositories** step, select **Override the default branch name for new repositories** and ensure the default branch name is **main**.
> 1. On the **Choosing the SSH executable** step, select **Use external OpenSSH**.
> 1. On the **Choosing HTTPS transport backend** step, select **Use the native Windows Secure Channel library**.
>
> Wait for the installation to complete and restart PowerShell for environment
> changes to take effect.

```Console
exit
```

```Console
cls
```

### # Configure symbolic link (e.g. for bash shell)

```PowerShell
Push-Location C:\NotBackedUp\Public\Toolbox\cmder\vendor

cmd /c mklink /J git-for-windows "C:\Program Files\Git"

Pop-Location
```

### # Configure Git to use SourceGear DiffMerge

```PowerShell
git config --global diff.tool diffmerge

git config --global difftool.diffmerge.cmd  '"C:/NotBackedUp/Public/Toolbox/DiffMerge/x64/sgdm.exe \"$LOCAL\" \"$REMOTE\"'
```

#### Reference

**Git for Windows (MSysGit) or Git Cmd**\
From <[https://sourcegear.com/diffmerge/webhelp/sec__git__windows__msysgit.html](https://sourcegear.com/diffmerge/webhelp/sec__git__windows__msysgit.html)>

```PowerShell
cls
```

## # Install GitHub Desktop

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\GitHub\GitHubDesktopSetup-x64.exe"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

## # Install GitKraken

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Axosoft\GitKrakenSetup-10.5.0.exe"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

## # Install and configure Node.js

### # Install Node.js

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\node.js\node-v22.12.0-x64.msi"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
>
> Wait for the installation to complete. Restart PowerShell for the change to the PATH environment variable to take effect.

```Console
exit
```

### # Change NPM file locations to avoid issues with redirected folders

```PowerShell
notepad "C:\Program Files\nodejs\node_modules\npm\npmrc"
```

> [!file] C:\\Program Files\\nodejs\\node_modules\\npm\\npmrc
>
> ```Text
>  ;prefix=${APPDATA}\npm
>  prefix=${LOCALAPPDATA}\npm
>  cache=${LOCALAPPDATA}\npm-cache
>  ``` 

```PowerShell
cls
```

## # Install PowerShell modules

### # Install posh-git module (e.g. for Powerline Git prompt customization)

#### # Install NuGet package provider (to bypass prompt when installing posh-git module)

```PowerShell
Install-PackageProvider NuGet -MinimumVersion '2.8.5.201' -Force
```

#### # Trust PSGallery repository (to bypass prompt when installing posh-git module)

```PowerShell
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
```

### # Install PowerShell for GitHub API

```PowerShell
Install-Module -Name PowerShellForGitHub
```

#### # Install posh-git module

```PowerShell
Install-Module -Name 'posh-git'
```

```PowerShell
cls
```

## # Install development tools

```PowerShell
cls
```

### # Install Postman

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Postman\Postman-win64-7.13.0-Setup.exe"

Start-Process -FilePath $setupPath -Wait
```

```PowerShell
cls
```

### # Install Wireshark

```PowerShell
$setupPath = "\\TT-FS01\Products\Wireshark\Wireshark-win64-3.0.7.exe"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
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

> [!important] **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

## # Install Paint.NET

```PowerShell
& "\\TT-FS01\Products\Paint.NET\paint.net.4.2.8.install.zip"
```

> [!important] **Important**
>
> Wait for the installation to complete.

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

## # Download PowerShell help files

```PowerShell
Update-Help
```

## Install updates using Windows Update

> [!note] Note
>
> Repeat until there are no updates available for the computer.

## Disk Cleanup

```PowerShell
cls
```

### # Delete C:\\Windows\\SoftwareDistribution folder (4.7 GB)

```PowerShell
Stop-Service wuauserv

Remove-Item C:\Windows\SoftwareDistribution -Recurse
```

```PowerShell
cls
```

## # Enter a product key and activate Windows

```PowerShell
slmgr /ipk {product key}
```

> [!note] Note
>
> When notified that the product key was set successfully, click **OK**.

```Console
slmgr /ato
```

## Configure profile for TECHTOOLBOX\\jjameson

> [!important] Important
>
> Login as TECHTOOLBOX\\jjameson

### Disable background apps

1. Open **Windows Settings**.
1. In the **Windows Settings** window, select **Privacy**.
1. On the **Privacy** page, select **Background apps**.
1. On the **Background apps** page, disable the following apps from running in
   the background:
   - **3D Viewer**
   - **Calculator**
   - **Camera**
   - **Feedback Hub**
   - **Get Help**
   - **Maps**
   - **Microsoft Photos**
   - **Microsoft Solitaire Collection**
   - **Microsoft Store**
   - **Mobile Plans**
   - **Movies & TV**
   - **Office**
   - **OneNote**
   - **Paint 3D**
   - **People**
   - **Print 3D**
   - **Snip & Sketch**
   - **Sticky Notes**
   - **Tips**
   - **Voice Recorder**
   - **Your Phone**

#### Issue: Photos app consumes high CPU

```PowerShell
cls
```

### # Configure e-mail and name for Git

```PowerShell
git config --global user.email "jjameson@technologytoolbox.com"
git config --global user.name "Jeremy Jameson"
```

### # Configure Git to use SourceGear DiffMerge

```PowerShell
git config --global diff.tool diffmerge

git config --global difftool.diffmerge.cmd  '"C:/NotBackedUp/Public/Toolbox/DiffMerge/x64/sgdm.exe \"$LOCAL\" \"$REMOTE\"'
```

#### Reference

**Git for Windows (MSysGit) or Git Cmd**\
From <[https://sourcegear.com/diffmerge/webhelp/sec__git__windows__msysgit.html](https://sourcegear.com/diffmerge/webhelp/sec__git__windows__msysgit.html)>

```PowerShell
cls
```

### # Configure personal access token for GitHub PowerShell module

Set-GitHubAuthentication

```PowerShell
cls
```

### # Install GitHub Desktop

```PowerShell
$setupPath = "\\TT-FS01\Products\GitHub\GitHubDesktopSetup.exe"

Start-Process -FilePath $setupPath -Wait
```

> [!important] **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

### # Add NPM "global" location to PATH environment variable

```PowerShell
Set-ExecutionPolicy RemoteSigned -Scope Process -Force

C:\NotBackedUp\Public\Toolbox\PowerShell\Add-PathFolders.ps1 `
    -Folders "$env:LOCALAPPDATA\npm" `
    -EnvironmentVariableTarget User
```

> **Important**
>
> Restart PowerShell for the change to PATH environment variable to take effect.

```Console
exit
```

### Install global NPM packages

> **Important**
>
> Install global NPM packages using a non-elevated instance of PowerShell (to
> avoid issues when subsequently running the npm install command as a "normal"
> user).

---

**Non-elevated** PowerShell instance

```PowerShell
cls
```

### # Install Angular CLI

```PowerShell
npm install --global --no-optional @angular/cli@7.3.9
```

### # Install Bitwarden CLI

```PowerShell
npm install --global @bitwarden/cli
```

### # Install Create React App

```PowerShell
npm install --global --no-optional create-react-app@3.2.0
```

### # Install rimraf

```PowerShell
npm install --global --no-optional rimraf@3.0.0
```

### # Install Yeoman, Gulp, and web app generator

```PowerShell
npm install --global --no-optional yo gulp-cli generator-webapp
```

---

#### Reference

**Web app generator**\
From <[https://www.npmjs.com/package/generator-webapp](https://www.npmjs.com/package/generator-webapp)>

## Install Docker for Windows

### Reference

[Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/)

### Install Windows Subsystem for Linux (WSL)

#### Reference

[How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

```PowerShell
cls
```

#### # Enable Windows Subsystem for Linux

```PowerShell
wsl --install
```

```PowerShell
cls
```

#### # Enable Virtual Machine feature

```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all `
    /norestart
```

#### Download and install Linux kernel update package

[WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

```PowerShell
cls
```

#### # Set WSL 2 as default version

```PowerShell
wsl --set-default-version 2
```

#### Install Linux distribution of choice

[Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)

### Install Docker Desktop

[Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/175267/Docker%20Desktop%20Installer.exe)

### Install additional Visual Studio Code extensions for Docker development

#### Install extension: Docker

```Text
ext install ms-azuretools.vscode-docker
```

#### Install extension: Kubernetes

```Text
ext install ms-kubernetes-tools.vscode-kubernetes-tools
```

#### Install extension: Remote - Containers

```Text
ext install ms-vscode-remote.remote-containers
```

#### Install extension: Remote - WSL

```Text
ext install ms-vscode-remote.remote-wsl
```

### Issue: Docker images consume large amount of disk space

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F3/5CCB258F8ECC8934921DD9F2FC5A3CBEDF7C91F3.png)

#### Move Docker images from C: drive (SSD) to F: drive (HDD)

#### Quit Docker Desktop

#### # Move Docker Desktop storage (VHDX) used for images

```PowerShell
wsl --shutdown

wsl --export docker-desktop-data docker-desktop-data.tar

wsl --unregister docker-desktop-data

mkdir E:\NotBackedUp\jjameson\docker-desktop-data

wsl --import docker-desktop-data E:\NotBackedUp\jjameson\docker-desktop-data `
    .\docker-desktop-data.tar --version 2
```

#### References

[How to move ext4.vhdx to a non system disk?](https://github.com/docker/for-win/issues/7348)

[how to move the vhdx of wsl2 to other disk](https://github.com/MicrosoftDocs/WSL/issues/412)

```PowerShell
cls
```

## # Connect to GitHub and GitLab using SSH

### # Generate SSH key

```Shell
ssh-keygen -t ed25519 -C "jjameson@technologytoolbox.com"
```

> [!note] Note
>
> When prompted to **Enter file in which to save the key**, press **Enter**.
> This accepts the default file location.
>
> Type a passphrase to secure the SSH key.

### Add SSH key to ssh-agent

---

**TT-W11-DEV-02** - Run as administrator

#### # Start ssh-agent as a background process

Get-Service -Name ssh-agent | Set-Service -StartupType Manual

Start-Service -Name ssh-agent

---

```PowerShell
cls
```

#### # Add SSH private key to ssh-agent

```PowerShell
$file = Resolve-Path ~/.ssh/id_ed25519 | select -ExpandProperty Path

ssh-add $file
```

> [!note] Note
>
> When prompted, type the secure passphrase previously specified for the SSH
> key.

```PowerShell
cls
```

### # Add SSH key to GitHub account

#### # Copy SSH public key to clipboard

```PowerShell
Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard
```

#### Add SSH public key to GitHub

1. Browse to
   [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new).
1. In the **Title** box, type **TT-W11-DEV-02**.
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. Click **Add SSH key**.

#### Add SSH public key to GitLab

1. Browse to
   [https://gitlab.com/-/profile/keys](https://gitlab.com/-/profile/keys).
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. In the **Title** box, type **TT-W11-DEV-02**.
1. Click **Add key**.

**TODO:**

## Share printer

![(screenshot)](https://assets.technologytoolbox.com/screenshots/01/2EDCC101189FC9A8E4E5FD9205D12E8EB82B2F01.png)

Click **Change Sharing Options**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/94/39D600ED528C73CAE3772FDA8A9E263E29CBF094.png)

## Other stuff that may need to be done

```PowerShell
cls
```

### # Configure credential helper for Git

```PowerShell
git config --global credential.helper !"C:\\NotBackedUp\\Public\\Toolbox\\git-credential-winstore.exe"
```

```PowerShell
cls
```

### # Install OpenCV

```PowerShell
setx -m OPENCV_DIR "C:\Program Files\OpenCV-3.4.6\opencv\build\x64\vc15"
```

#### # Add OpenCV bin folder to PATH environment variable

```PowerShell
Set-ExecutionPolicy RemoteSigned -Scope Process -Force

$openCVPathFolder = "%OPENCV_DIR%\bin"

C:\NotBackedUp\Public\Toolbox\PowerShell\Add-PathFolders.ps1 `
    -Folders $openCVPathFolder `
    -EnvironmentVariableTarget Machine
```

```PowerShell
cls
```

### # Install Microsoft InfoPath 2013

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> [!note] Note
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Office 2013" `
    + "\infopath_4753-1001_x86_en-us.exe"

Start-Process `
    -FilePath $setupPath `
    -Wait
```

> [!important] **Important**
>
> Wait for the installation to complete.

### Install Android SDK (for debugging Chrome on Samsung Galaxy)

#### Reference

[http://stackoverflow.com/a/24410867](http://stackoverflow.com/a/24410867)

#### Install Samsung USB driver

#### Install Java SE Development Kit 8u66

[http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

#### Install Android "SDK Tools Only"

[http://developer.android.com/sdk/index.html#Other](http://developer.android.com/sdk/index.html#Other)

#### Install Android SDK Platform-tools

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1E/75601D9C9EF795A16D815FE580D6571F20B23C1E.png)

#### Detect devices

```Console
cd C:\Program Files (x86)\Android\android-sdk\platform-tools

adb.exe devices
```

### Install Apple iTunes

### Install ASP.NET ViewState Helper 2.0.1

### Install Inkscape 0.48

### # Clone repository - Training

```PowerShell
mkdir src\Repos

cd src\Repos

git clone https://techtoolbox.visualstudio.com/DefaultCollection/_git/Training
```

### # Install ng-demos

#### # Install package dependencies

```PowerShell
npm install -g node-inspector
```

#### # Clone repo

```PowerShell
cd Training

git clone https://github.com/johnpapa/ng-demos.git
```

#### # Install packages

```PowerShell
cd ng-demos\modular

npm install
```

#### # Run dev build

```PowerShell
gulp serve-dev-debug
```

```PowerShell
cls
```

## # Create and configure Caelum website

### # Set environment variables

```PowerShell
[Environment]::SetEnvironmentVariable(
  "CAELUM_URL",
  "http://www-local.technologytoolbox.com",
  "Machine")
```

> **Important**
>
> Restart PowerShell for environment variable to be available.

```PowerShell
C:\NotBackedUp\Public\Toolbox\PowerShell\Add-Hostnames.ps1 `
    -IPAddress 127.0.0.1 `
    -Hostnames www-local.technologytoolbox.com
```

### # Rebuild website

```PowerShell
cd "C:\NotBackedUp\techtoolbox\Caelum\Main\Source\Deployment Files\Scripts"

& '.\Rebuild Website.ps1'
```
