---
created: 2024-12-07T17:44:14Z
modified: 2024-12-12T11:45:42Z
---

Yesterday I decided to finally let go of Windows 10 and proceeded to [[Documenting Infrastructure Environments|rebuild my primary desktop]] with [[Goodbye Windows 10, hello Windows 11|Windows 11]]. The process went mostly smooth, but [[OneDrive]] kept getting stuck on "Loading…" when I would sign in using my [[Hotmail]] account.

The steps documented in the following Microsoft Support article resolved the issue:

[Reset OneDrive - Microsoft Support](https://support.microsoft.com/en-us/office/reset-onedrive-34701e00-bf7b-42db-b960-84905399050c)

The issue still occurred after running `wsreset.exe`. Consequently, I ran the next recommended command:

```Console
%localappdata%\Microsoft\OneDrive\onedrive.exe /reset
```

This forced an upgrade of OneDrive and I was then able to sign in and sync the OneDrive folder.

Woohoo! 😊
