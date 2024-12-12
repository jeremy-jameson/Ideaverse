At some point since [[Nick Milo]] created his video [introducing hotkeys in Obsidian](https://www.youtube.com/watch?v=cDcoBMVJsvk), the folks over at Obsidian (the company) decided to change the defaults. For example, after installing Obsidian on my Windows PC, `Ctrl+E` was bound by default to **Toggle reading view**. Personally, as a new user of Obsidian, I don't find the reading view to be nearly as useful as toggling between **Source** and **Live Preview** modes.

Consequently, I customized the Obsidian hotkeys as follows:

| Action                          | Hotkey   |
| ------------------------------- | -------- |
| Toggle reading view             | `Ctrl+R` |
| Toggle Live Preview/Source mode | `Ctrl+E` |

❗**Important:** Custom hotkeys are saved in the `.obsidian/hotkeys.json` file within each Obsidian vault. If the vault is stored in a version control system (e.g. Git), then I recommend adding that file to the repository. This ensures the custom hotkeys work as expected across various environments and, more importantly, are not accidentally lost (e.g. deleting a local repository and subsequently cloning it again from version control).
