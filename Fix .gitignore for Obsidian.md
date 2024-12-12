## .gitignore changes

When I woke up this morning, I almost immediately started thinking about my `.gitignore` for Obsidian and decided to fix it.

Due to the fact that Obsidian stores a lot of configuration information in the **.obsidian/** folder -- such as hotkeys and plugins -- it seems like the best choice is to *include* everything in that folder by default (with a few specific exclusions) rather than *excluding* everything and trying to keep track of what *should* be specifically included.

As I discovered with custom hotkeys, the latter approach is likely to lead to configuration changes being lost over time. The former approach -- while somewhat risky if sensitive data within plugins were accidentally leaked through a public repository -- seems, overall, to be the far "safer" option.

### Reference

Christopher Patton provides a detailed explanation of various `.gitignore` options in the following pull request:

<https://github.com/github/gitignore/pull/4370>

## Include community plugins in source control (not just the plugin configuration)

Note that Obsidian stores the *list* of community plugins currently installed for the vault in the `.obsidian/community-plugins.json` file. However, it currently does not automatically install those plugins as necessary when opening the vault.

To understand the issues with this, consider the following scenario:

1. I am working in a new environment (e.g. a freshly built Linux VM) and consequently want to clone the [[Ideaverse]] vault from GitHub (so I can view or add some notes without switching back to my Windows desktop).
1. I run `git clone https://github.com/jeremy-jameson/Ideaverse.git`,
1. I install Obsidian on the Linux VM.
1. I open the vault in the folder cloned from GitHub.

While the `community-plugins.json` file specifies the plugins previously installed on my Windows desktop, Obsidian ignores the fact that they are missing on the Linux VM. As a result, my notes do not render as expected (for example, due to the **Dataview** plugin not being installed).

It would be ideal if Obsidian prompted to install any missing plugins specified in `community-plugins.json`, but since it currently does *not*, I am changing `.gitignore` to include the entire contents of the `.obsidian/plugins` folder.
