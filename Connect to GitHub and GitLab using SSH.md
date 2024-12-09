```PowerShell
cls
```
## # Connect to GitHub and GitLab using SSH

### # Generate SSH key

```Shell
ssh-keygen -t ed25519 -C "jjameson@technologytoolbox.com"
```

> **Note:**
>
> When prompted to **Enter file in which to save the key**, press **Enter**. This accepts the default file location.
>
> Type a passphrase to secure the SSH key.

### Add SSH key to ssh-agent

---

**STORM** - Run as administrator

#### # Start ssh-agent as a background process

Get-Service -Name ssh-agent | Set-Service -StartupType Manual

Start-Service -Name ssh-agent

---

```PowerShell
cls
```

#### # Add SSH private key to ssh-agent

```PowerShell
$file = Resolve-Path ~/.ssh/id_ed25519 | select -ExpandProperty Path

ssh-add $file
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
Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard
```

#### Add SSH public key to GitHub

1. Browse to [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new).
1. In the **Title** box, type **STORM**.
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. Click **Add SSH key**.

#### Add SSH public key to GitLab

1. Browse to [https://gitlab.com/-/profile/keys](https://gitlab.com/-/profile/keys).
1. In the **Key** box, paste the SSH public key previously copied the clipboard.
1. In the **Title** box, type **STORM**.
1. Click **Add key**.
