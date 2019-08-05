# iOS

### iOS beta deployment

In summary, the steps are like the below

1. Create a certificate
2. Create a provisioning profile based on the certificate
3. Create an Archive
4. Validate it and then upload it to iTunes Connect
5. Upload the app to TestFlight with Application Loader

#### Application Loader

Sign in with app-specific password. 

To generate app-specific password

1. Go to [https://appleid.apple.com/account/manage](https://appleid.apple.com/account/manage)
2. In security, Generate Password

#### What is Provisioning Profile

> A provisioning profile is a collection of digital entities that uniquely ties developers and devices to an authorised iPhone Development Team and enables a device to be used for testing

![](.gitbook/assets/image%20%281%29.png)

To submit to testflight, you need distribution profile

> TestFlight apps submitted to iTunesConnect need to be signed with an App Store Distribution Profile. TestFlight no longer accepts apps submitted with an Ad Hoc profile.

#### Certificate

1. Star Creating certificate, selecting iOS Distribution on App Store and AdHoc
2. Create a certificate signing request
   1. Launch Keychain Access
   2. Keychain Access &gt; Certificate Assistant &gt; Request a Certificate from a Certificate Authority.
   3. Enter an email address in the User Email Address field.
   4. In the Common Name field, enter a name for the key \(for example, Gita Kumar Dev Key\).
   5. Leave the CA Email Address field empty
   6. Choose “Saved to disk”, and click Continue.
3. Choose signing request file and continue
4. Now the certificate is ready. download and install it by double-clicking it.

![](.gitbook/assets/image%20%2810%29.png)

#### Profile

1. Start and select Distribution for TestFlight distribution
2. Select app id for distribution
3. Generate a provisioning profile by selecting a certificate
4. Download and install the profile

#### Deploy In XCode

1. Create an app icon, if it doesn't have it yet. use [app icon generator](https://appiconmaker.co/).
2. Product &gt; Archive
3. Upload and select profile
4. 
