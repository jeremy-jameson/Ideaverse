---
created: 2024-12-09T17:58:51Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

## # Disk Cleanup

### # Delete C:\\Windows\\SoftwareDistribution folder

```PowerShell
Stop-Service wuauserv

Remove-Item C:\Windows\SoftwareDistribution -Recurse
```
