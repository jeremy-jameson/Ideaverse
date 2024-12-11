## .gitignore changes

When I woke up this morning, I almost immediately started thinking about my `.gitignore` for Obsidian and decided to fix it.

Due to the fact that Obsidian stores a lot of configuration information in the **.obsidian/** folder -- such as hotkeys and plugins -- it seems like the best choice is to *include* everything in that folder by default (with a few specific exclusions) rather than *excluding* everything and trying to keep track of what *should* be specifically included.

As I discovered with custom hotkeys, the latter approach is likely to lead to configuration changes being lost over time. The former approach -- while somewhat risky if sensitive data within plugins were accidentally leaked through a public repository -- seems, overall, to be the far "safer" option.
### Reference

Christopher Patton provides a detailed explanation of various `.gitignore` options in the following pull request:

https://github.com/github/gitignore/pull/4370