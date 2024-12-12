```PowerShell
cls
```

## # Install Visual Studio Code

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Visual Studio Code" `
    + "\VSCodeSetup-x64-1.40.2.exe"

$arguments = "/silent" `
    + " /mergetasks='!runcode,addcontextmenufiles,addcontextmenufolders" `
        + ",addtopath'"

Start-Process `
    -FilePath $setupPath `
    -ArgumentList $arguments `
    -Wait
```

> **Important**
>
> Wait for the installation to complete.

### Issue

**Installer doesn't disable launch of VScode even when installing with /mergetasks=!runcode**\
From <[https://github.com/Microsoft/vscode/issues/46350](https://github.com/Microsoft/vscode/issues/46350)>

### Modify Visual Studio Code shortcut to use custom extension and user data locations

```Console
"C:\Program Files\Microsoft VS Code\Code.exe" --extensions-dir "C:\NotBackedUp\vscode-data\extensions" --user-data-dir "C:\NotBackedUp\vscode-data\user-data"
```

### Install Visual Studio Code extensions

#### Install extension: Azure Resource Manager Tools

#### Install extension: Beautify

#### TODO: Install extension: C/C++

```PowerShell
ms-vscode.cpptools
```

#### Install extension: C&#35;

#### Install extension: Debugger for Chrome

#### TODO: Install extension: ES7 React/Redux/GraphQL/React-Native snippets

```Text
dsznajder.es7-react-js-snippets
```

#### Install extension: ESLint

#### Install extension: GitLens - Git supercharged

#### Install extension: markdownlint

#### Install extension: PowerShell

#### Install extension: Prettier - Code formatter

#### Install extension: SQL Server (mssql)

#### Install extension: TSLint

#### TODO: Install extension: VBScript

```Text
darfka.vbscript
```

#### Install extension: vscode-icons

#### Install extension: XML Tools

---

> **Notes**
>
> Potential issue when using both Beautify and Prettier extensions:
>
> **Prettier & Beautify**\
> From <[https://css-tricks.com/prettier-beautify/](https://css-tricks.com/prettier-beautify/)>
>
> HTML formatting issue with Prettier:
>
> **Add the missing option to disable crappy Prettier VSCode HTML formatter \#636**\
> From <[https://github.com/prettier/prettier-vscode/issues/636](https://github.com/prettier/prettier-vscode/issues/636)>

---

#### Configure Visual Studio Code settings

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
