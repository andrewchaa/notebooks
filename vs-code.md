# VS Code

## Table of Contents

* [Set up](vs-code.md#table-of-contents)
* [Keyboard Shortcuts](vs-code.md#keyboard-shortcuts)
* [Debugging](vs-code.md#debugging)

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

### Extensions to install

**JavaScript**

* Live Server
* Bracket Pair Colorizer

## Keyboard Shortcuts

| Keys | Description |
| :--- | :--- |
| ctrl - | go back to your previous location |
| ctrl + 1 | focus on editor |
| ctrl + 1, ctrl + 2 | switch across split panes |
| shft + alt | column select |
| shft + alt + f | format document. You can format your JSON document |

## Debugging

* Install React Native Tools on VS Code
* Go to Debug tab. Add Configuration. It'll create launch.json
* Turn off Remote Debugging on Simulator. 
* On Debug tab, select Attach to packager, as the simulator is already running. 
* On Debug console, you will see all the outputs.

## Troubleshooting

#### React Native Debug JS Remotely Error window.deltaUrlToBlobUrl is not a function

Just close the debugger tab and refresh the simulator

