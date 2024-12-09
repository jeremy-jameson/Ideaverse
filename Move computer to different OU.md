
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
