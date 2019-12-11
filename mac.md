---
description: Setting your new shiny Mac!
---

# Mac Set-up

## Resource

* [https://github.com/nicolashery/mac-dev-setup](https://github.com/nicolashery/mac-dev-setup)

## System preferences

### Track Pad / Keyboard

* Trackpad &gt; Tap to click
* Keyboard &gt; Key Repeat &gt; Fast \(all the way to the right\)
* Keyboard &gt; Delay Until Repeat &gt; Short \(all the way to the right\)
* Dock &gt; Automatically hide and show the Dock

### Change computer name

1. Click on the **Apple** icon on the upper left corner of your **Mac** screen. ...
2. In the System Preferences window, select the blue "Sharing" folder.
3. In the "Sharing" window, simply click into the text box next to "Computer **Name**" at the top and enter your new computer **name**.

### Control Strip on Touch Bar

1. Click on System Preferences in the drop down menu.
2. Click on Keyboard.
3. Click on the Keyboard tab.
4. Click on Customize Control Strip.
5. Drag Screen Lock to the bottom, so that you can lock the screen

### Three finger drag

System Preference &gt; Accessibility &gt; Pointer Control &gt; Trackpad Options

![](.gitbook/assets/image%20%2822%29.png)

## zsh

```text
$ brew install zsh
```

{% embed url="https://sourabhbajaj.com/mac-setup/iTerm/zsh.html" %}

{% embed url="https://github.com/sorin-ionescu/prezto" %}



## Apps

### Rectangle

[https://rectangleapp.com/](https://rectangleapp.com/)

### HomeBrew: [https://brew.sh/](https://brew.sh/)

```bash
brew --verion
```

### Fira Code

{% embed url="https://github.com/tonsky/FiraCode" %}

### VS Code

{% embed url="https://code.visualstudio.com/docs/?dv=osx" %}

If it doesn't open on Mac Catalina, go to Security & Privacy and choose "Open Anyway"

#### PATH

Open shell command by pressing SHFT + CMD + P and "install 'code' command in PATH to open code from terminal. 

#### Preferences &gt; Settings

```bash
{
  "editor.tabSize": 2,
  "editor.rulers": [80],
  "editor.fontFamily": "Fira Code",
  "editor.fontLigatures": true,
  "files.insertFinalNewline": true,
  "files.trimTrailingWhitespace": true,
  "workbench.editor.enablePreview": false
}
```

### Git

```bash
brew install git
```

#### .gitconfig

```bash
[alias]
  # Show verbose output about tags, branches or remotes
  tags = tag -l
  branches = branch -a
  remotes = remote -v
  # Pretty log output
  hist = log --graph --pretty=format:'%Cred%h%Creset %s%C(yellow)%d%Creset %Cgreen(%cr)%Creset [%an]' --abbrev-commit --date=relative

[color]
  # Use colors in Git commands that are capable of colored output when
  # outputting to the terminal. (This is the default setting in Git ≥ 1.8.4.)
  ui = auto
[color "branch"]
  current = yellow reverse
  local = yellow
  remote = green
[color "diff"]
  meta = yellow bold
  frag = magenta bold
  old = red bold
  new = green bold
[color "status"]
  added = yellow
  changed = green
  untracked = cyan
```

Basic config

```bash
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```

Global .gitignore

```bash
# Folder view configuration files
.DS_Store
Desktop.ini

# Thumbnail cache files
._*
Thumbs.db

# Files that might appear on external disks
.Spotlight-V100
.Trashes
```

```bash
git config --global core.excludesfile ~/.gitignore
```

Install Xcode

Xcode: [https://apps.apple.com/us/app/xcode/id497799835?mt=12](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

Make sure to install Command Line Tools

![](.gitbook/assets/image%20%285%29.png)

#### Fork: [https://git-fork.com/](https://git-fork.com/)

## Languages

### Ruby

#### rbenv

```text
brew install rbenv
```

After installation, add the following line to your `.zshrc`:

```text
eval "$(rbenv init -)"
```

And reload it with:

```text
source ~/.zshrc
```

#### Install ruby

```bash
rbenv install --list
rbenv install 2.6.5
```

#### Node

```bash
brew install node
node -–version
npm --versrion
```

#### React / React Native

Watchman: [https://facebook.github.io/watchman/](https://facebook.github.io/watchman/)

```bash
brew install watchman
watchman –version
```

React Native

```bash
npm install -g react-native-cli
react-native --version
```

Install Cocoapods: [https://cocoapods.org/](https://cocoapods.org/)

```bash
sudo gem install cocoapods
pod --version
```

Install JDK

```bash
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
java -version
javac -version
```

Install Android Studio: [https://developer.android.com/studio](https://developer.android.com/studio)

Set up Android Studio SDK

![](.gitbook/assets/image%20%2815%29.png)

Set Android Studio Environment Variables

```bash
export ANDROID_HOME=/usr/local/share/android-sdk
export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
export PATH=${PATH}:$ANDROID_HOME/tools/bin
export PATH=${PATH}:$ANDROID_HOME/emulator
```

#### react native debugger

```text
brew update && brew cask install react-native-debugger
```

