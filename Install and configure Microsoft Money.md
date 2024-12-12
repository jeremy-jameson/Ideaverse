```PowerShell
cls
```

## # Install and configure Microsoft Money

### # Install Microsoft Money

```PowerShell
net use \\TT-FS01\Products /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Microsoft\Money 2008\USMoneyBizSunset.exe"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.

### Patch Money DLL to avoid crash when importing transactions

1. Open DLL in hex editor:

   ```Console
   C:\NotBackedUp\Public\Toolbox\HxD\HxD.exe "C:\Program Files (x86)\Microsoft Money Plus\MNYCoreFiles\mnyob99.dll"
   ```

1. Make the following changes:

   File offset **003FACE8**: Change **85** to **8D**\
   File offset **003FACED**: Change **50** to **51**\
   File offset **003FACF0**: Change **FF** to **85**\
   File offset **003FACF6**: Change **E8** to **B9**

#### Reference

**Microsoft Money crashes during import of account transactions or when changing a payee of a downloaded transaction**\
From <[http://blogs.msdn.com/b/oldnewthing/archive/2012/11/13/10367904.aspx](http://blogs.msdn.com/b/oldnewthing/archive/2012/11/13/10367904.aspx)>

```PowerShell
cls
```

### # Configure firewall rule to block Microsoft Money quote service

```PowerShell
New-NetFirewallRule `
    -Name "BlockMicrosoftMoney" `
    -DisplayName "Block Microsoft Money" `
    -Direction Outbound `
    -Program "%ProgramFiles%\Microsoft Money Plus\MNYCoreFiles\msmoney.exe" `
    -Action Block
```

#### Reference

**Eliminating the "online updating"delays and errors when opening Money**\
From <[https://microsoftmoneyoffline.wordpress.com/2016/06/06/eliminating-the-online-updatingdelays-and-errors-when-opening-money/](https://microsoftmoneyoffline.wordpress.com/2016/06/06/eliminating-the-online-updatingdelays-and-errors-when-opening-money/)>

```PowerShell
cls
```

### # Create custom invoice template

```PowerShell
& "C:\Program Files (x86)\Microsoft Money Plus\MNYCoreFiles\tmplbldr.exe"
```

In the **New Template** window:

1. Ensure **Create new template** is selected and click **Next**.
1. In the **Name** box, type **Technology Toolbox** and click **Next**.
1. Ensure **Portrait** is selected and click **Finish**.
1. On the **File** menu, click **Save**.

> **Note**
>
> The new invoice template is created in the following folder:
>
> C:\\Users\\All Users\\Microsoft\\Money\\17.0\\Invoice

```PowerShell
cls
```

#### # Overwrite file with custom template

```PowerShell
Copy-Item `
    'C:\Users\jjameson\OneDrive - Technology Toolbox\InvoiceTemplate.htm' `
    'C:\Users\All Users\Microsoft\Money\17.0\Invoice\usr19.htm'
```

#### Configure default invoice template

Edit invoice listing (2008Invoice.ntd) to move the "Technology Toolbox" invoice to the top (so it is selected by default).

```Console
Notepad "C:\Users\All Users\Microsoft\Money\17.0\Invoice\2008Invoice.ntd"
```

#### Reference

**Sunset Home & Business - Invoice issues**\
From <[https://microsoftmoneyoffline.wordpress.com/sunset-home-business-invoices/](https://microsoftmoneyoffline.wordpress.com/sunset-home-business-invoices/)>

### Install and configure MSMoneyQuotes

```PowerShell
cls
```

### # Install Python 2 (dependency for PocketSense)

#### # Install Python (using default options)

```PowerShell
net use \\TT-FS01\IPC$ /USER:TECHTOOLBOX\jjameson
```

> **Note**
>
> When prompted, type the password to connect to the file share.

```PowerShell
$setupPath = "\\TT-FS01\Products\Python\python-2.7.15.amd64.msi"

Start-Process -FilePath $setupPath -Wait
```

> **Important**
>
> Wait for the installation to complete.

```PowerShell
cls
```

#### # Add Python folders to PATH environment variable

```PowerShell
Set-ExecutionPolicy RemoteSigned -Scope Process -Force

$pythonPathFolders = 'C:\Python27\', 'C:\Python27\Scripts'

C:\NotBackedUp\Public\Toolbox\PowerShell\Add-PathFolders.ps1 `
    -Folders $pythonPathFolders `
    -EnvironmentVariableTarget Machine
```

### # Copy PocketSense files

```PowerShell
$source = "\\TT-FS01\Users$\jjameson\My Documents\Finances\MS Money\PocketSense"
$destination = "C:\BackedUp\MS Money\PocketSense"

robocopy $source $destination /E
```
