---
created: 2024-12-09T16:27:04Z
modified: 2024-12-09T16:27:04Z
---

### Rename computer and join domain

```PowerShell
$computerName = "STORM"

Rename-Computer -NewName $computerName -Restart
```

Wait for the VM to restart and then execute the following command to join the **TECHTOOLBOX** domain:

```PowerShell
Add-Computer -DomainName corp.technologytoolbox.com -Restart
```
