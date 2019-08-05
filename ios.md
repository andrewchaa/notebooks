# iOS

### iOS beta deployment

#### What is Provisioning Profile

> A provisioning profile is a collection of digital entities that uniquely ties developers and devices to an authorised iPhone Development Team and enables a device to be used for testing

![](.gitbook/assets/image.png)

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

![](.gitbook/assets/image%20%289%29.png)

#### Profile

1. Start and select Distribution for TestFlight distribution
2. Select app id for distribution
3. Generate a provisioning profile by selecting a certificate
4. Download and install the profile

#### In XCode

1. Select the project. Select the target \(iPhone app, not tv OS\). by default the project is selected.

