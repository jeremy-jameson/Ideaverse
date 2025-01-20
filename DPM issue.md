---
created: 2025-01-19T12:21:50Z
modified: 2025-01-20T11:04:52Z
---

```PowerShell

New-ItemProperty `
    -Path "HKLM:\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Agent" `
    -Name "ConnectionNoActivityTimeout" `
    -PropertyType DWord `
    -Value 7200

New-ItemProperty `
    -Path "HKLM:\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Agent" `
    -Name "ConnectionNoActivityTimeoutForNonCCJobs" `
    -PropertyType DWord `
    -Value 7200

Restart-Service DPMRA
```
