# VS Code

## Set up

#### Set tab-to-space to 2 spaces

 You can customize this easily via these three settings for **Windows** in menu _File_ → _Preferences_ → _User Settings_ and for **Mac** in menu _Code_ → _Preferences_ → _Settings_ or `⌘,`:

```javascript
// The number of spaces a tab is equal to. This setting is overridden
// based on the file contents when `editor.detectIndentation` is true.
"editor.tabSize": 4,

// Insert spaces when pressing Tab. This setting is overriden
// based on the file contents when `editor.detectIndentation` is true.
"editor.insertSpaces": true,

// When opening a file, `editor.tabSize` and `editor.insertSpaces`
// will be detected based on the file contents. Set to false to keep
// the values you've explicitly set, above.
"editor.detectIndentation": false
```

#### Install recommended extensions

* PowerShell
* Terraform
* markdownlint
* Beautify
* jshint
* ESLint

### Keyboard Shortcuts

| Keys | Description |
| :--- | :--- |
| ctrl + 1, ctrl + 2 | switch across split panes |
| shft + alt | column select |
| shft + alt + f | format document. You can format your JSON document |

