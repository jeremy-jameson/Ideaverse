---
created: 2024-12-09T17:44:18Z
modified: 2025-01-02T12:58:46Z
---

```PowerShell
cls
```

## # Connect to GitHub and GitLab using SSH

### # Generate SSH key

```PowerShell
[string] $keyName = "jjameson+2025-05@technologytoolbox.com"
[string] $keyPath = [System.IO.Path]::Combine(
    (Resolve-Path "~"),
    ".ssh",
    $keyName)

ssh-keygen -t ed25519 -f "$keyPath" -C $keyName
```

> **Note:**
>
> When prompted, type a passphrase to secure the SSH key.

### Add SSH key to ssh-agent

---

**PowerShell** - Run as administrator

#### # Start ssh-agent as a background process

```PowerShell
Get-Service -Name ssh-agent | Set-Service -StartupType Manual

Start-Service -Name ssh-agent
```

---

```PowerShell
cls
```

#### # Add SSH private key to ssh-agent

```PowerShell
ssh-add $keyPath
```

> **Note:**
>
> When prompted, type the secure passphrase previously specified for the SSH key.

```PowerShell
cls
```

### # Add SSH key to GitHub account

#### # Copy SSH public key to clipboard

```PowerShell
[string] $publicKeyPath = $keyPath + ".pub"

Get-Content $publicKeyPath | Set-Clipboard
```

#### Add SSH public key to GitHub

1. Browse to [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new).
1. In the **Title** box, type the name of the SSH key (`jjameson+2025-05@technologytoolbox.com`).
1. Ensure **Key type** is set to **Authentication Key**.
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. Click **Add SSH key**.

#### Add SSH public key to GitLab

1. Browse to [https://gitlab.com/-/profile/keys](https://gitlab.com/-/profile/keys).
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. In the **Title** box, type the name of the SSH key (`jjameson+2025-05@technologytoolbox.com`).
1. Click **Add key**.

## # Configure commit signing in Git

### # Add SSH key to GitHub account

#### # Copy SSH public key to clipboard

```PowerShell
Get-Content $publicKeyPath | Set-Clipboard
```

#### Add SSH public key to GitHub

1. Browse to [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new).
1. In the **Title** box, type the name of the SSH key (`jjameson+2025-05@technologytoolbox.com`).
1. Ensure **Key type** is set to **Signing Key**.
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. Click **Add SSH key**.

### # Configure Git to sign every commit with SSH key

```PowerShell
# Configure Git to use SSH for signing, instead of the default (GPG)
git config --global gpg.format ssh

# Configure Git to use SSH key for signing
[string] $publicKey = Get-Content $publicKeyPath
[string] $signingKey = "key::" + $publicKey

git config --global user.signingkey $signingKey

# Configure Git to sign every commit
git config --global commit.gpgsign true
```

### # Configure "allowed signers" for Git

```PowerShell
[string] $allowedSignersPath = [System.IO.Path]::Combine(
    (Resolve-Path "~"),
    ".ssh",
    "allowed_signers")

[string] $gitUserEmail = (git config --get user.email)

Set-Content -Path $allowedSignersPath -Value "$gitUserEmail $publicKey"

git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```
