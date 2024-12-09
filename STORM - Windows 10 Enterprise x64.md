
Monday, July 2, 2018\
11:20 AM

```Text
12345678901234567890123456789012345678901234567890123456789012345678901234567890
```

### [[Install Windows 10 Enterprise (x64)]]

Install DaVinci Resolve

### [[Rename computer and join domain]]

### [[Move computer to different OU]]
### Login as local administrator account

### [[Install NVIDIA display driver]]

### [[Configure networking]]

### [[Disable 'Automatically detect proxy settings' in Internet Explorer]]

### [[Configure storage]]

### [[Install software for HP Photosmart 6515]]

### [[Disable background apps]]

### [[Enable firewall rules for Disk Management]]

### [[Select power scheme#Select 'High performance' power scheme]]

### [[Enable PowerShell remoting]]

### [[Create default folders]]

### [[Configure cmder shortcut in Windows Explorer]]

## [[Install and configure Hyper-V]]
### Install Microsoft Office

### Install OneNote 2016

### Install Google Chrome

### [[Install Bitwarden]]

## [[Install and configure Microsoft Money]]

## [[Configure backup]]

## [[Install Microsoft SQL Server 2017]]

## [[Install Visual Studio Code]]

## [[Install Visual Studio 2019]]

### [[Install .NET Core 2.2 SDK]]

### [[Install .NET Framework 4.6.2 Developer Pack]]

## [[Install and configure Git]]

## [[Install GitHub Desktop]]

## [[Install GitKraken]]

## [[Install and configure Node.js]]

## [[Install PowerShell modules]]

## [[Install development tools]]

## [[Configure development environment]]

## # Configure hosts file

```PowerShell
Set-ExecutionPolicy Bypass -Scope Process -Force

C:\NotBackedUp\Public\Toolbox\PowerShell\Add-Hostnames.ps1 `
    -IPAddress 10.1.20.99 `
    -Hostnames EXT-VS2017-DEV2, www-local.technologytoolbox.com
```

```PowerShell
cls
```

## # Download PowerShell help files

```PowerShell
Update-Help
```

## Install updates using Windows Update

> **Note**
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

**Note:** When notified that the product key was set successfully, click **OK**.

```Console
slmgr /ato
```

## Configure profile for TECHTOOLBOX\\jjameson

> **Important**
>
> Login as TECHTOOLBOX\\jjameson

### Disable background apps

1. Open **Windows Settings**.
2. In the **Windows Setttings** window, select **Privacy**.
3. On the **Privacy** page, select **Background apps**.
4. On the **Background apps** page, disable the following apps from running in the background:
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

> **Important**
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
> Install global NPM packages using a non-elevated instance of PowerShell (to avoid issues when subsequently running the npm install command as a "normal" user).

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

```PowerShell
cls
```

## # Upgrade Git

### # Install Git

```PowerShell
$setupPath = "\\TT-FS01\Products\Git\Git-2.24.1.2-64-bit.exe"

Start-Process -FilePath $setupPath -Wait
```

On the **Choosing the default editor used by Git** step, select **Use the Nano editor by default**.

> **Important**
>
> Wait for the installation to complete and restart PowerShell for environment changes to take effect.

## [[Replace DPM server (TT-DPM05 -- TT-DPM06)]]

## Install Docker for Windows

### Reference

[Install Docker Desktop on Windows](https://docs.docker.com/docker-for-windows/install/)

### Install Windows Subsystem for Linux (WSL)

#### Reference

[Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

```PowerShell
cls
```

#### # Enable Windows Subsystem for Linux

```PowerShell
dism.exe /online /enable-feature `
    /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
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

[Docker Desktop for Windows](https://desktop.docker.com/win/stable/amd64/Docker%20Desktop%20Installer.exe)

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

## [[Connect to GitHub and GitLab using SSH]]

```PowerShell
cls
```

## # Upgrade Git

```PowerShell
$setupPath = "\\TT-FS01\Products\Git\Git-2.33.0.2-64-bit.exe"

Start-Process -FilePath $setupPath -Wait
```

On the **Choosing the default editor used by Git** step, select **Use the Nano editor by default**.

> **Important**
>
> Wait for the installation to complete and restart PowerShell for environment changes to take effect.

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

### [[Install Microsoft InfoPath 2013]]

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

## [[Create and configure Caelum website]]