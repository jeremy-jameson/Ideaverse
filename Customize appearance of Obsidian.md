---
created: 2024-12-11T14:29:08Z
modified: 2024-12-12T11:45:42Z
---

## Configure Minimal theme in Obsidian

Reference: [Minimal Documentation > Get started](https://minimal.guide/home#Get+started)

### 1. Install Minimal theme 

[Open in Obsidian](obsidian://show-theme?name=Minimal) or follow the steps below:

1. Open Obsidian Settings
1. Go to `Appearance` and click `Manage`
1. Search for **Minimal** and click `Install and use`

### 2. Install the companion plugin 

[Open in Obsidian](obsidian://show-plugin?id=obsidian-minimal-settings) or follow the steps below:

1. Go to `Community Plugins` and turn off `Safe mode`
1. Search for **Minimal Theme Settings** and click `Install`, then `Enable`

### 3. Install optional related plugins 

- [Style Settings](https://minimal.guide/plugins/style-settings) allows you to create custom [Color schemes](https://minimal.guide/features/color-schemes) and tweak Minimal with more fine-grained control.
- [Hider](https://minimal.guide/plugins/hider) hides UI elements such as window frame, scrollbars, tooltips, etc.

## Customize heading colors

One of the issues for me, personally, after applying the Minimal theme in Obsidian is the reduced sizes of headings. This makes it difficult for me to easily distinguish the various levels of headings, as illustrated in the screenshot below:

![Obsidian - Heading default colors with Minimal theme](https://i.imgur.com/XWrqOyc.png)

Consequently, I customize the heading colors using [CSS snippets](https://help.obsidian.md/Extending+Obsidian/CSS+snippets). With just a little bit of CSS, the various heading levels now appear in different colors:

```css
body {
    --h1-color: var(--color-purple);
    --h2-color: var(--color-blue);
    --h3-color: var(--color-green);
    --h4-color: var(--color-yellow);
    --h5-color: var(--color-orange);
    --h6-color: var(--color-red);
}
```

![Obsidian - Heading custom colors with Minimal theme](https://i.imgur.com/W18v6eE.png)

**Note:** The CSS rules use [extended color variables](https://docs.obsidian.md/Reference/CSS+variables/Foundations/Colors#Extended+colors) in order to support both light and dark modes.
