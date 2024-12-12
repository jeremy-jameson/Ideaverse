---
created: 2024-12-09T17:05:13Z
modified: 2024-12-09T17:05:13Z
---

## Install and configure Hyper-V

### Enable Hyper-V

```PowerShell
cls
```

### # Configure VM storage

```PowerShell
mkdir D:\NotBackedUp\VMs

Set-VMHost -VirtualMachinePath D:\NotBackedUp\VMs
```

### # Create virtual switches

```PowerShell
$interfaceAlias = "LAN"

New-VMSwitch `
    -Name $interfaceAlias `
    -NetAdapterName $interfaceAlias `
    -AllowManagementOS $true
```

### # Enable jumbo frames on virtual switches

```PowerShell
Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Set-NetAdapterAdvancedProperty `
    -Name "vEthernet ($interfaceAlias)" `
    -DisplayName "Jumbo Packet" `
    -RegistryValue 9014

Start-Sleep -Seconds 5

ping TT-FS01 -f -l 8900
```
