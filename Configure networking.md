### # Configure networking

```PowerShell
$interfaceAlias = "LAN"
```

#### # Rename network connections

```PowerShell
Get-NetAdapter -Physical | select InterfaceDescription

$interfaceDescription = "Intel(R) Gigabit CT Desktop Adapter"

Get-NetAdapter -InterfaceDescription $interfaceDescription |
    Rename-NetAdapter -NewName $interfaceAlias
```

#### # Enable jumbo frames

```PowerShell
Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Set-NetAdapterAdvancedProperty `
    -Name $interfaceAlias `
    -DisplayName "Jumbo Packet" `
    -RegistryValue 9014

Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Start-Sleep -Seconds 5

ping TT-FS01 -f -l 8900
```