---
created: 2024-12-23T20:31:50Z
modified: 2024-12-26T17:11:03Z
---

A quick note on formatting large numbers in PowerShell.

```PowerShell
[int64] $oneTerabyte = 1TB

Write-Host ("1 terabyte is " + $oneTerabyte + " bytes" `
    + " -- which is very difficult to read")
```

```text
1 terabyte is 1099511627776 bytes -- which is very difficult to read
```

```PowerShell
[string] $formattedNumber = "{0:n0}" -f 1TB

Write-Host ("1 terabyte is " + $formattedNumber + " bytes" `
    + " -- which is slightly easier to read")
```

```text
1 terabyte is 1,099,511,627,776 bytes -- which is slightly easier to read
```

```PowerShell
Function Format-ByteNumber() {
    Param ([int64]$Number)
    If     ($Number -ge 1PB) {"{0:n2}" -f ($Number / 1PB) + " PB"}
    ElseIf ($Number -ge 1TB) {"{0:n2}" -f ($Number / 1TB) + " TB"}
    ElseIf ($Number -ge 1GB) {"{0:n2}" -f ($Number / 1GB) + " GB"}
    ElseIf ($Number -ge 1MB) {"{0:n2}" -f ($Number / 1MB) + " MB"}
    ElseIf ($Number -ge 1KB) {"{0:n2}" -f ($Number / 1KB) + " KB"}
    ElseIf ($Number -ge 0)   {"{0:n0}" -f $Number + " B"}
    Else                     {""}
}

$formattedNumber = Format-ByteSize 1TB

Write-Host ("1 terabyte is " + $formattedNumber `
    + " -- which is very easy to read")
```

```text
1 terabyte is 1.00 TB -- which is very easy to read
```

While `Format-ByteNumber` works, I prefer the following:

```PowerShell
function Format-SizeInBytes {
    param(
        [Parameter(Mandatory = $true, ValueFromPipeline = $true)]
        [uint64] $SizeInBytes
    )

    [float] $modifiedSize = $SizeInBytes

    [string[]] $units = "B", "KB", "MB", "GB", "TB", "PB"
    [int] $index = 0

    while ($modifiedSize -ge 1024 -and $index -lt ($units.Count - 1)) {
        $modifiedSize /= 1024
        $index++
    }

    [string] $formattedNumber = [string]::Empty

    if ($SizeInBytes -lt 1KB) {
        $formattedNumber = "{0:n0}" -f $modifiedSize
    }
    else {
        $formattedNumber = "{0:n2}" -f $modifiedSize
    }

    return ($formattedNumber + " " + $units[$index])
}
```
