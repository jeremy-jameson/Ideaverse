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
