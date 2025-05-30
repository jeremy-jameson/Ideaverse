---
created: 2025-05-29T12:15:18Z
modified: 2025-05-30T11:54:46Z
---

As part of the effort to improve security, I decided to reset the passwords on the numerous demo accounts that I created over the years. When I subsequently need to login using one of these accounts, I'll use my administrator account to reset the password (and save it in [[Bitwarden]]).

## Set random password for individual user

```PowerShell
$username = 'agreen'

$password = Invoke-WebRequest `
    -Uri https://www.dinopass.com/password/strong |
    Select-Object -ExpandProperty content

[int] $randomNumber = Get-Random -Minimum 100 -Maximum 999

$password += $randomNumber.ToString()

Set-ADAccountPassword `
    -Identity $username `
    -Reset `
    -NewPassword (ConvertTo-SecureString `
	    -AsPlainText $password -Force -Verbose) `
    –PassThru
```

## Set random password for CONTOSO domain users

```PowerShell
cls

Get-ADUser -Filter {Created -gt '12/7/2016'} |
	where { $_.SamAccountName -notin (
		'aparks', 'aparks-admin', 'DefaultAccount') } |
    foreach {
        $username = $_.SamAccountName
        $password = [string]::Empty

        while ($password.Length -lt 11) {
            $password = Invoke-WebRequest `
		            -Uri https://www.dinopass.com/password/strong |
                Select-Object -ExpandProperty content
        }

        [int] $randomNumber = Get-Random -Minimum 100 -Maximum 999

        $password += $randomNumber.ToString()

        Write-Debug "Username: $username"
        Write-Debug "Password: $password"

        Set-ADAccountPassword `
            -Identity $username `
            -Reset `
            -NewPassword (ConvertTo-SecureString `
	            -AsPlainText $password -Force -Verbose) `
            –PassThru
    }
```

## Set random password for FABRIKAM domain users

```PowerShell
cls

Get-ADUser -Filter {Created -gt '5/10/2013'} |
    where { $_.SamAccountName -notlike 'r-*' } |
    where { $_.SamAccountName -notlike 'SM_*' } |
    where { $_.SamAccountName -notin ('DefaultAccount', 'EXTRANET$') } |
    foreach {
        $username = $_.SamAccountName
        $password = [string]::Empty

        while ($password.Length -lt 11) {
            $password = Invoke-WebRequest `
		            -Uri https://www.dinopass.com/password/strong |
                Select-Object -ExpandProperty content
        }

        [int] $randomNumber = Get-Random -Minimum 100 -Maximum 999

        $password += $randomNumber.ToString()

        Write-Debug "Username: $username"
        Write-Debug "Password: $password"

        Set-ADAccountPassword `
            -Identity $username `
            -Reset `
            -NewPassword (ConvertTo-SecureString `
	            -AsPlainText $password -Force -Verbose) `
            –PassThru
    }
```

## Set random password for EXTRANET domain users

```PowerShell
cls

Get-ADUser -Filter {Created -gt '10/14/2013'} |
	where { $_.SamAccountName -notlike '*-admin' } |
	where { $_.SamAccountName -notin ('DefaultAccount') } |
    foreach {
        $username = $_.SamAccountName
        $password = [string]::Empty

        while ($password.Length -lt 11) {
            $password = Invoke-WebRequest `
		            -Uri https://www.dinopass.com/password/strong |
                Select-Object -ExpandProperty content
        }

        [int] $randomNumber = Get-Random -Minimum 100 -Maximum 999

        $password += $randomNumber.ToString()

        Write-Debug "Username: $username"
        Write-Debug "Password: $password"

        Set-ADAccountPassword `
            -Identity $username `
            -Reset `
            -NewPassword (ConvertTo-SecureString `
	            -AsPlainText $password -Force -Verbose) `
            –PassThru
    }
```

## Set random password for TECHTOOLBOX domain users

```PowerShell
cls

Get-ADUser -Filter {Created -gt '8/16/2006 9:03:09 AM'} |
    where { $_.SamAccountName -notlike '*-admin' } |
    where { $_.SamAccountName -notin (
	    'DefaultAccount', 'EXTRANET$', 'jjameson-fabric') } |
    where { $_.SamAccountName -notlike 'MSOL_*' } |
    where { $_.SamAccountName -notlike 's-*' } |
    foreach {
        $username = $_.SamAccountName
        $password = [string]::Empty

        while ($password.Length -lt 12) {
            $password = Invoke-WebRequest `
		            -Uri https://www.dinopass.com/password/strong |
                Select-Object -ExpandProperty content
        }

        [int] $randomNumber = Get-Random -Minimum 100 -Maximum 999

        $password += $randomNumber.ToString()

        Write-Debug "Username: $username"
        Write-Debug "Password: $password"

        Set-ADAccountPassword `
            -Identity $username `
            -Reset `
			-NewPassword (ConvertTo-SecureString `
				-AsPlainText $password -Force -Verbose) `
            –PassThru
    }
```
