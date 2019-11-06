# Mac

## Set up for Development

Install HomeBrew: [https://brew.sh/](https://brew.sh/)

```
brew --verion
```

Install Node.js

```
brew install node
node -–version
npm --versrion
```

Install Watchman: [https://facebook.github.io/watchman/](https://facebook.github.io/watchman/)

```
brew install watchman
watchman –version
```

Install React Native

```
npm install -g react-native-cli
react-native --version
```

Install Xcode

Xcode: [https://apps.apple.com/us/app/xcode/id497799835?mt=12](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

Make sure to install Command Line Tools

![](.gitbook/assets/image%20%284%29.png)

Install Cocoapods: [https://cocoapods.org/](https://cocoapods.org/)

```
sudo gem install cocoapods
pod --version
```

Install JDK

```
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
java -version
javac -version
```

Install Android Studio: [https://developer.android.com/studio](https://developer.android.com/studio)

Set up Android Studio SDK

![](.gitbook/assets/image%20%2813%29.png)

Set Android Studio Environment Variables

```
export ANDROID_HOME=/usr/local/share/android-sdk
export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
export PATH=${PATH}:$ANDROID_HOME/tools/bin
export PATH=${PATH}:$ANDROID_HOME/emulator
```

