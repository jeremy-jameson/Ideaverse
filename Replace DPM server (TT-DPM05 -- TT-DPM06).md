## Replace DPM server (TT-DPM05 --> TT-DPM06)

```PowerShell
cls
```

### # Update DPM server

```PowerShell
cd 'C:\Program Files\Microsoft Data Protection Manager\DPM\bin\'

.\SetDpmServer.exe -dpmServerName TT-DPM06.corp.technologytoolbox.com
```

---

**TT-ADMIN04** - DPM Management Shell

```PowerShell
cls
```

### # Attach DPM agent

```PowerShell
$productionServer = 'STORM'

.\Attach-ProductionServer.ps1 `
    -DPMServerName TT-DPM06 `
    -PSName $productionServer `
    -Domain TECHTOOLBOX `
    -UserName jjameson-admin
```

---

That doesn't work...

> Error:\
> Data Protection Manager Error ID: 307\
> The protection agent operation failed because DPM detected an unknown DPM protection agent on storm.corp.technologytoolbox.com.
>
> Recommended action:\
> Use Add or Remove Programs in Control Panel to uninstall the protection agent from storm.corp.technologytoolbox.com, then reinstall the protection agent and perform the operation again.

```PowerShell
cls
```

### # Remove DPM 2019 Agent Coordinator

```PowerShell
msiexec /x `{356B3986-6B7D-4513-B72D-81EB4F43ADE6`}
```

```PowerShell
cls
```

### # Remove DPM 2019 Protection Agent

```PowerShell
msiexec /x `{CC6B6758-3A68-4BBA-9D61-1F3278D6A7EA`}
```

> **Important**
>
> Restart the computer to complete the removal of the DPM agent.

```PowerShell
Restart-Computer
```

### # Install DPM 2019 agent

```PowerShell
$installerPath = "\\TT-FS01\Products\Microsoft\System Center 2019" `
    + "\DPM\Agents\DPMAgentInstaller_x64.exe"

$installerArguments = "TT-DPM06.corp.technologytoolbox.com"

Start-Process `
    -FilePath $installerPath `
    -ArgumentList "$installerArguments" `
    -Wait
```

---

**TT-ADMIN04** - DPM Management Shell

```PowerShell
cls
```

### # Attach DPM agent

```PowerShell
$productionServer = 'STORM'

.\Attach-ProductionServer.ps1 `
    -DPMServerName TT-DPM06 `
    -PSName $productionServer `
    -Domain TECHTOOLBOX `
    -UserName jjameson-admin
```

---

### Add client to protection group in DPM

```PowerShell
cls
```

### # Configure antivirus on DPM protected server

#### # Disable real-time monitoring by Windows Defender for DPM server

```PowerShell
[array] $excludeProcesses = Get-MpPreference | select -ExpandProperty ExclusionProcess

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
