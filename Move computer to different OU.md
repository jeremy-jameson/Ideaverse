---
created: 2024-12-09T16:32:16Z
modified: 2024-12-09T16:32:53Z
---


---

**FOOBAR16** - Run as administrator

```PowerShell
cls
```

## Move computer to "Development" OU

```PowerShell
$vmName = "STORM"

$targetPath = ("OU=Workstations,OU=Resources,OU=Development" `
    + ",DC=corp,DC=technologytoolbox,DC=com")

Get-ADComputer $vmName | Move-ADObject -TargetPath $targetPath
```

---
