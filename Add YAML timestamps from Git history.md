---
created: 2024-12-12T12:59:41Z
modified: 2024-12-12T13:29:06Z
---

## Get Dates from Git.ps1

```powershell
$repoPath = 'C:\BackedUp\GitHub\jeremy-jameson\Ideaverse'

pushd $repoPath

Get-ChildItem -Recurse '*.md' |
    foreach {
        [string] $localTimestamp = (
            git log --follow --format=%ad --date iso-strict $_.FullName |
                select -Last 1)

        [string] $utcCreated = ''
        [string] $utcModified = ''

        If ($localTimestamp.Length -gt 0)
        {
            [DateTime] $created = [DateTime]::Parse($localTimestamp)

            $utcCreated = $created.ToUniversalTime().ToString('s') + 'Z'

            $localTimestamp = (
                git log --follow --format=%ad --date iso-strict $_.FullName |
                    select -First 1)

            [DateTime] $modified = [DateTime]::Parse($localTimestamp)

            $utcModified = $modified.ToUniversalTime().ToString('s') + 'Z'
        }

        $result = New-Object PSObject
            
        $result |
            Add-Member `
                -MemberType NoteProperty `
                -Name Path `
                -Value $_.FullName

        $result |
            Add-Member `
                -MemberType NoteProperty `
                -Name Created `
                -Value $utcCreated

        $result |
            Add-Member `
                -MemberType NoteProperty `
                -Name Modified `
                -Value $utcModified

        $result
    } |
    sort Created |
    Export-Csv Files.csv -NoTypeInformation

popd
```

### Reference

[Finding the date/time a file was first added to a Git repository](https://stackoverflow.com/a/2390382)

## Files.csv

```text
Path,Created,Modified
C:\BackedUp\GitHub\jeremy-jameson\Ideaverse\README.md,2024-12-05T18:33:42Z,2024-12-12T11:45:42Z
C:\BackedUp\GitHub\jeremy-jameson\Ideaverse\Ideaverse.md,2024-12-05T19:55:25Z,2024-12-12T11:45:42Z
C:\BackedUp\GitHub\jeremy-jameson\Ideaverse\Alex Kretzschmar.md,2024-12-05T21:13:54Z,2024-12-12T11:45:42Z
C:\BackedUp\GitHub\jeremy-jameson\Ideaverse\Pocket Casts.md,2024-12-05T21:25:18Z,2024-12-12T11:45:42Z
C:\BackedUp\GitHub\jeremy-jameson\Ideaverse\Mandy.md,2024-12-05T21:45:36Z,2024-12-12T11:45:42Z
```

## Add YAML Frontmatter to Markdown Files.ps1

```powershell
$repoPath = 'C:\BackedUp\GitHub\jeremy-jameson\Ideaverse'

pushd $repoPath

Import-Csv Files.csv |
    foreach {
        # Define the new content to prepend
        $newContent = "---`n" `
            + "created: " + $_.Created + "`n" `
            + "modified: " + $_.Modified + "`n" `
            + "---`n" `
            + "`n"

        # Read the existing content of the file
        $existingContent = Get-Content -Path $_.Path -Raw

        # Combine the new content with the existing content
        $combinedContent = $newContent + $existingContent

        # Write the combined content back to the file
        Set-Content -Path $_.Path -Value $combinedContent.TrimEnd()
    }

popd
```
