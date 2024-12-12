## Configure profile for TECHTOOLBOX\\jjameson

> **Important**
>
> Login as TECHTOOLBOX\\jjameson

### Disable background apps

1. Open **Windows Settings**.
1. In the **Windows Settings** window, select **Privacy**.
1. On the **Privacy** page, select **Background apps**.
1. On the **Background apps** page, disable the following apps from running in the background:
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
