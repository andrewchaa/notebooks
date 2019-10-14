# Detox

## Getting Started

#### Update brew and install node

```bash
brew update && brew install node
```

#### Install [AppSimUtils](https://github.com/wix/AppleSimulatorUtils)

```bash
brew tap wix/brew
brew install applesimutils
```

Verify the installation by running applesimutils in terminal

#### Add detox to the project

```bash
npm install detox --save-dev
npm i -g detox-cli
```

**Add Detox config to package.json**

```javascript
"detox": {
  "configurations": {
    "ios.sim.debug": {
      "binaryPath": "ios/build/navienapp/Build/Products/Debug-iphonesimulator/navienapp.app",
      "build": "xcodebuild -project ios/navienapp.xcodeproj -scheme navienapp -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build",
      "type": "ios.simulator",
      "name": "iPhone 7"
    }
  }
},
```

The project build will fail if you have installed dependencies by Cocoapod. Update the build script

```javascript
"build": "xcodebuild -workspace ios/navienapp.xcworkspace -scheme navienapp -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build",
```

**Install a test runner**

```bash
npm install jest --save-dev
detox init -r jest
```

\*\*\*\*

