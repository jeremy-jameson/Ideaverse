---
created: 2024-12-12T09:22:45Z
modified: 2024-12-12T16:31:19Z
---

## Recommended settings

For the vast majority of the **Linter** plugin settings, use the configuration recommended by [[Lou Plummer]]:

[Don't Be Afraid to Use the Linter Plugin in #Obsidian | Amerpie by Lou Plummer](https://amerpie.lol/2024/06/05/dont-be-afraid.html)

Modifications to Lou's recommendations are documented in the following sections.

## Configure YAML Timestamp settings

1. On the **YAML** tab:
	1. Enable **YAML Timestamp**.
	1. Enable **Date Created**.
	1. For **Date Created Key**, specify **created**.
	1. For **Date Created Source of Truth**, select **YAML frontmatter**.
	1. Enable **Date Modified**.
	1. For **Date Modified Key**, specify **modified**.
	1. For **Date Modified Source of Truth**, select **Changes in Obsidian**.
	1. For **Format**, specify **YYYY-MM-DDTHH:mm:ss\[Z\]**.
	1. Enable **Convert Local Time to UTC**.

![Obsidian - Linter YAML Timestamp initial configuration](https://i.imgur.com/wD3XB9T.png)

## Use "lazy" number style for ordered lists

I prefer to have all list items prefixed by "1. " because it simplifies `git diff` when reordering items.

On the **Content** tab:

	1. Enable **Ordered List Style**.

	2. For **Number Style**, select **Lazy**.

Example:

```markdown
1. Move the images
1. Rename the image files
1. Commit the changes to source control
1. Upload the images
```
