---
created: 2025-01-01T21:52:53Z
modified: 2025-01-01T22:12:51Z
---

While doing some cleanup, I discovered a duplicate [[Toolbox]] folder that I copied from my old desktop ([[STORM - Windows 10 Enterprise x64]]). While I was *pretty sure* this only contained duplicate files, I wanted to be certain I didn't have any "one-off" files or pending changes in there -- so, like I've done in the past, I compared file hashes, just in case.

```PowerShell
cls
```

## # Remove files from "duplicate" folder that exist in "source" folder

```PowerShell
$sourceFolderPath = "C:\NotBackedUp\Public\Toolbox"

$duplicateFolderPath = "D:\NotBackedUp\STORM\NotBackedUp\Public\Toolbox"

Get-ChildItem -File -Recurse $sourceFolderPath |
    foreach {
        $path1 = $_.FullName;
        $path2 = $_.FullName.Replace($sourceFolderPath, $duplicateFolderPath)

        if ((Test-Path $path2) -eq $true) {
            $hash1 = Get-FileHash -Algorithm SHA1 $path1 |
                select -ExpandProperty Hash

            $hash2 = Get-FileHash -Algorithm SHA1 $path2 |
                select -ExpandProperty Hash

            if ($hash1 -eq $hash2) {
                Remove-Item -Force $path2
            }
        }
    }
```

## # Recursively remove empty folders from "duplicate" folder

```PowerShell
# A script block (anonymous function) that will remove empty folders
# under a root folder, using tail-recursion to ensure that it only
# walks the folder tree once. -Force is used to be able to process
# hidden files/folders as well.
$tailRecursion = {
    param(
        $Path
    )
    foreach ($childDirectory in Get-ChildItem -Force -LiteralPath $Path -Directory) {
        & $tailRecursion -Path $childDirectory.FullName
    }
    $currentChildren = Get-ChildItem -Force -LiteralPath $Path
    $isEmpty = $currentChildren -eq $null
    if ($isEmpty) {
        Write-Verbose "Removing empty folder at path '${Path}'."
        Remove-Item -Force -LiteralPath $Path
    }
}

& $tailRecursion -Path $duplicateFolderPath
```

### Reference

[How to recursively remove all empty folders in PowerShell?](https://stackoverflow.com/a/28637537)
