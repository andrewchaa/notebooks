# iOS

## iOS beta deployment

In summary, the steps are like the below

1. Understand [Provisioning Profile](ios.md#what-is-provisioning-profile)
2. Create a [certificate](ios.md#certificate)
3. [Create a provisioning profile](ios.md#creating-a-profile) based on the certificate
4. [Update info.plist](ios.md#update-info-plist)
5. Create an Archive
6. Validate it 
7. Upload it to iTunes Connect by "Distribute App"
8. Choose TestFlight and [check if the new version is there](ios.md#check-the-new-version-on-app-connect)
9. Add yourself to tester

If you don't have any user, add testers on [Users and Access](https://appstoreconnect.apple.com/access/users)

### What is Provisioning Profile

> A provisioning profile is a collection of digital entities that uniquely ties developers and devices to an authorised iPhone Development Team and enables a device to be used for testing

![](.gitbook/assets/image%20%283%29.png)

To submit to testflight, you need distribution profile

> TestFlight apps submitted to iTunesConnect need to be signed with an App Store Distribution Profile. TestFlight no longer accepts apps submitted with an Ad Hoc profile.

### Create a Certificate

1. Start Creating certificate, selecting iOS Distribution on App Store and AdHoc
2. Upload a Certificate Signing Request
   1. Create a certificate signing request
      1. Launch Keychain Access
      2. Keychain Access &gt; Certificate Assistant &gt; Request a Certificate from a Certificate Authority.
      3. Enter an email address in the User Email Address field.
      4. In the Common Name field, enter a name for the key \(for example, Gita Kumar Dev Key\).
      5. Leave the CA Email Address field empty
      6. Choose “Saved to disk”, and click Continue.
   2. Choose signing request file and continue
3. Now the certificate is ready. 
4. Download and install it by double-clicking it.
5. Make sure to save a backup copy of your private and public keys somewhere secure

![](.gitbook/assets/image%20%2822%29.png)

### Creating a Profile

1. Register a new provisioning Profile on AppConnect
2. Distribution &gt; AppStore
3. Click "Continue"
4. Select an available AppId and Continue
5. Select an available certificate and continue
6. Enter Provisioning Profile Name and click "Generate"
7. Download
8. Double click the following file to install your Provisioning Profile.

### Downloading a profile

When you lost the provisioning profile, you would need to download it again. 

* Go to [https://developer.apple.com](https://developer.apple.com)
* Select Profile
* Click on download
* Import from xcode project settings &gt; general
* if the profile's appId doesn't match what you have on the project, change the project's appId. Go to project settings. select target. General. Update Bundle Identifier

### Update info.plist

1. Open info.plist and update the version number
   1. CFBundleShortVersionString: increase the version
   2. CFBundleVersion: same
2. Make sure you have 
   1. Privacy - Photo Library Usage Description
   2. Privacy - Location Always Usage Description
   3. Privacy - Location When In Use Usage Description

### Deploy In XCode

1. Create an app icon, if it doesn't have it yet. use [app icon generator](https://appiconmaker.co/).
2. Product &gt; Archive
3. Upload and select profile

### Check the new version on App Connect

* Check if the new version is there
* If the new version doesn't appear, it might have some issues on info.plist. The common reason is lack of permission description.

```markup
<key>NSLocationWhenInUseUsageDescription</key>
<string>To correctly locate installation place</string>
<key>NSMicrophoneUsageDescription</key>
<string>Need microphone access for uploading audios</string>
<key>NSCameraUsageDescription</key>
<string>To scan barcode</string>
<key>NSSpeechRecognitionUsageDescription</key>
<string>To understand user speech</string>
<key>NSPhotoLibraryAddUsageDescription</key>
<string>To load image gallery</string>
```

### Create a new certificate

1. Go to [developer portal](https://developer.apple.com).
2. Create a [certificate signing request](ios.md#create-a-certificate-signing-request)
3. Choose the csr file and click on Continue
4. download the certificate

### Create a certificate signing request

1. Launch Keychain Access located in `/Applications/Utilities`.
2. Choose Keychain Access &gt; Certificate Assistant &gt; Request a Certificate from a Certificate Authority.
3. In the Certificate Assistant dialog, enter an email address in the User Email Address field.
4. In the Common Name field, enter a name for the key \(for example, Gita Kumar Dev Key\).
5. Leave the CA Email Address field empty.
6. Choose “Saved to disk”, and click Continue.

![](.gitbook/assets/image%20%288%29.png)

## Set iOS App Icon for App Store Publish

* Select Images.xcassets
* Click on AppIcon
* Add icons

## Troubleshooting

#### Installing additional require components

Required for react native build

Go to Preferences &gt; Component

![](.gitbook/assets/image%20%282%29.png)

