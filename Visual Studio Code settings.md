---
created: 2024-12-29T21:43:22Z
modified: 2024-12-29T23:20:28Z
---

A quick note to capture my VS Code settings.

```JSON
{
  "diffEditor.ignoreTrimWhitespace": false,
  "editor.formatOnSave": true,
  "editor.renderWhitespace": "boundary",
  "editor.rulers": [80],
  "files.trimTrailingWhitespace": true,
  "git.autofetch": true,
  "git.suggestSmartCommit": false,
  "html.format.wrapLineLength": 80,
  "prettier.disableLanguages": ["html"],
  "prettier.proseWrap": "always",
  "terminal.integrated.shell.windows": "C:\\windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "workbench.editor.highlightModifiedTabs": true,
  "workbench.iconTheme": "vscode-icons",
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "files.trimTrailingWhitespace": false
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "vs-kubernetes": {
    "vscode-kubernetes.minikube-path.windows": "C:\\Users\\jjameson\\.vs-kubernetes\\tools\\minikube\\windows-amd64\\minikube.exe",
    "vscode-kubernetes.helm-path.windows": "C:\\Users\\jjameson\\.vs-kubernetes\\tools\\helm\\windows-amd64\\helm.exe"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "mssql.connections": [
    {
      "server": "{{put-server-name-here}}",
      "database": "{{put-database-name-here}}",
      "user": "{{put-username-here}}",
      "password": ""
    }
  ]
}
```
