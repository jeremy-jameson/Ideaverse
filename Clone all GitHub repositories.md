---
created: 2024-12-29T20:08:27Z
modified: 2024-12-31T16:06:25Z
---

After building a new development VM ([[TT-W11-DEV-02]]), I wanted to quickly clone all of my repos.

## Reference

[How to clone all repos at once from GitHub?](https://stackoverflow.com/a/68770988)

## Install GitHub CLI

```shell
winget install --id GitHub.cli
```

## Authenticate GitHub CLI

```shell
gh auth login
```

```PowerShell
cls
```

## # Clone personal repositories

```PowerShell
pushd C:\BackedUp\GitHub\jeremy-jameson

(gh repo list jeremy-jameson --limit 4000 --json 'url' |
	ConvertFrom-Json) |
	foreach { git clone "$($_.url)" }

popd
```

```PowerShell
cls
```

## # Clone organization repositories

```PowerShell
pushd C:\BackedUp\GitHub\technology-toolbox

(gh repo list technology-toolbox --limit 4000 --json 'url' |
	ConvertFrom-Json) |
	foreach { git clone "$($_.url)" }

popd
```
