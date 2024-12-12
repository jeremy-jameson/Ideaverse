---
created: 2024-12-09T17:12:44Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

## # Install Microsoft SQL Server 2017

### # Create folders for Distributed Replay Client

```PowerShell
mkdir "D:\NotBackedUp\Microsoft SQL Server\DReplayClient\WorkingDir\"
mkdir "D:\NotBackedUp\Microsoft SQL Server\DReplayClient\ResultDir\"
```

### # Install SQL Server

```PowerShell
$imagePath = ("\\TT-FS01\Products\Microsoft\SQL Server 2017" `
    + "\en_sql_server_2017_developer_x64_dvd_11296168.iso")

$imageDriveLetter = (Mount-DiskImage -ImagePath $ImagePath -PassThru |
    Get-Volume).DriveLetter

& ("$imageDriveLetter" + ":\setup.exe")
```

On the **Feature Selection** step, click **Select All** and then clear the checkbox for **PolyBase Query Service for External Data** (since this requires the Java Runtime Environment to be installed).

On the **Server Configuration** page:

- For **SQL Server Database Engine**, change the **Startup Type** to **Manual**.
- For **SQL Server Analysis Services**, change the **Startup Type** to **Manual**.
- For **SQL Server Integration Services 14.0**, change the **Startup Type** to **Manual**.
- For **SQL Server Integration Services Scale Out Master 14.0**, change the **Startup Type** to **Manual**.
- For **SQL Server Integration Services Scale Out Worker 14.0**, change the **Startup Type** to **Manual**.

On the **Database Engine Configuration** page:

- On the **Server Configuration** tab, click **Add Current User**.
- On the **Data Directories** tab:
  - Change the **Data root directory** to **D:\\NotBackedUp\\Microsoft SQL Server\\**
  - Change the **Backup directory** to **Z:\\Microsoft SQL Server\\MSSQL14.MSSQLSERVER\\MSSQL\\Backup**

On the **Analysis Services Configuration** page:

- On the **Server Configuration** tab, click **Add Current User**.
- On the **Data Directories** tab:
  - Change the **Data directory** to **D:\\NotBackedUp\\Microsoft SQL Server\\MSAS14.MSSQLSERVER\\OLAP\\Data.**
  - Change the **Log file directory** to **D:\\NotBackedUp\\Microsoft SQL Server\\MSAS14.MSSQLSERVER\\OLAP\\Log.**
  - Change the **Temp directory** to **D:\\NotBackedUp\\Microsoft SQL Server\\MSAS14.MSSQLSERVER\\OLAP\\Temp.**
  - Change the **Backup directory** to **Z:\\NotBackedUp\\Microsoft SQL Server\\MSAS14.MSSQLSERVER\\OLAP\\Backup**.

On the **Distributed Replay Controller** page, click **Add Current User**.

On the **Distributed Replay Client** page:

- On the **Server Configuration** tab, click **Add Current User**.
  - Change the **Working Directory** to **D:\\NotBackedUp\\Microsoft SQL Server\\DReplayClient\\WorkingDir\\.**
  - Change the **Result Directory** to **D:\\NotBackedUp\\Microsoft SQL Server\\DReplayClient\\ResultDir\\.**

```PowerShell
cls
```

### # Remove installation media

```PowerShell
Dismount-DiskImage $imagePath
```

### # Install cumulative update for SQL Server

```PowerShell
& "\\TT-FS01\Products\Microsoft\SQL Server 2017\Patches\CU17\SQLServer2017-KB4515579-x64.exe"
```

```PowerShell
cls
```

### # Configure settings for SQL Server Agent job history log

#### # Do not limit size of SQL Server Agent job history log

```PowerShell
$sqlcmd = @"
EXEC msdb.dbo.sp_set_sqlagent_properties @jobhistory_max_rows=-1,
    @jobhistory_max_rows_per_job=-1
```

"@

```PowerShell
Invoke-Sqlcmd $sqlcmd -Verbose -Debug:$false

Set-Location C:
```

##### Reference

**SQL SERVER - Dude, Where is the SQL Agent Job History? - Notes from the Field \#017**\
From <[https://blog.sqlauthority.com/2014/02/27/sql-server-dude-where-is-the-sql-agent-job-history-notes-from-the-field-017/](https://blog.sqlauthority.com/2014/02/27/sql-server-dude-where-is-the-sql-agent-job-history-notes-from-the-field-017/)>

```PowerShell
cls
```

#### # Configure SQL Server maintenance

##### # Create database for SQL Server maintenance

```PowerShell
$sqlcmd = "CREATE DATABASE SqlMaintenance"

Invoke-Sqlcmd $sqlcmd -Verbose -Debug:$false

Set-Location C:
```

##### # Create maintenance table, stored procedures, and jobs

```PowerShell
$url = "https://raw.githubusercontent.com/technology-toolbox" `
    + "/sql-server-maintenance-solution/master/MaintenanceSolution.sql"

$tempFileName = [System.IO.Path]::GetTempFileName()

Invoke-WebRequest -Uri $url -OutFile $tempFileName

Invoke-Sqlcmd -InputFile $tempFileName -Verbose -Debug:$false

Set-Location C:

Remove-Item $tempFileName
```

##### # Configure schedules for SQL Server maintenance jobs

```PowerShell
$url = "https://raw.githubusercontent.com/technology-toolbox" `
    + "/sql-server-maintenance-solution/master/JobSchedules.sql"

$tempFileName = [System.IO.Path]::GetTempFileName()

Invoke-WebRequest -Uri $url -OutFile $tempFileName

Invoke-Sqlcmd -InputFile $tempFileName -Verbose -Debug:$false

Set-Location C:

Remove-Item $tempFileName
```

##### Reference

**SQL Server Backup, Integrity Check, and Index and Statistics Maintenance**\
From <[https://ola.hallengren.com/](https://ola.hallengren.com/)>
