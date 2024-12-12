---
created: 2024-12-09T17:45:30Z
modified: 2024-12-12T11:45:42Z
---

```PowerShell
cls
```

## # Create and configure Caelum website

### # Set environment variables

```PowerShell
[Environment]::SetEnvironmentVariable(
  "CAELUM_URL",
  "http://www-local.technologytoolbox.com",
  "Machine")
```

> **Important**
>
> Restart PowerShell for environment variable to be available.

```PowerShell
C:\NotBackedUp\Public\Toolbox\PowerShell\Add-Hostnames.ps1 `
    -IPAddress 127.0.0.1 `
    -Hostnames www-local.technologytoolbox.com
```

### # Rebuild website

```PowerShell
cd "C:\NotBackedUp\techtoolbox\Caelum\Main\Source\Deployment Files\Scripts"

& '.\Rebuild Website.ps1'
```
