# fastlane

## Installation

```bash
# install xcode commandline tool
xcode-select --install     

# install fastlane
brew cask install fastlane 

# add to path
code ~/.zshrc
add to path, $HOME/.fastlane/bin
```

#### Navigate to your project and run

Navigate to directory where your ios / android project file sits

```text
fastlane init
```

### iOS beta deployment

#### What is Provisioning Profile

> A provisioning profile is a collection of digital entities that uniquely ties developers and devices to an authorised iPhone Development Team and enables a device to be used for testing

![](.gitbook/assets/image.png)

To submit to testflight, you need distribution profile

> TestFlight apps submitted to iTunesConnect need to be signed with an App Store Distribution Profile. TestFlight no longer accepts apps submitted with an Ad Hoc profile.



To deploy an app as beta to TestFlight. The required steps are



#### Create a Provisioning Profile

* Go to [https://developer.apple.com](https://developer.apple.com) and create one
* Select distribution profile

#### Import the profile

* Open the project. Set the target to the app. Then General tab appear.
* Import the profile

#### Provisioning profile "app" doesn't include signing certificate "iPhone Distribution:"



```text
fastlane beta
```

**Conflicting provisioning settings**

* Set Code Signing Style to Automatic
* Set Code Signing Identity to iOS Developer

![](.gitbook/assets/image%20%288%29.png)

#### No profiles for 'org.deepeyes.navienapp' were found



#### Sign certificate for iOS



