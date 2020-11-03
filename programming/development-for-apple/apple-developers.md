# Code signing

![](../../.gitbook/assets/image%20%28156%29.png)

At the top, you’ll see the Apple Certificate Authority. There are other certificate authorities, but Apple is the only authority that will allow you to push your app to App Store Connect \(previously iTunes Connect\). App Store Connect is the gateway to the Apple store.

One of the first things you do as an iOS developer is to create a Signing Certificate. The Signing Certificate guarantees that you are you and that Apple knows about you. The process for doing this is explained completely in the Apple documentation. The gist is that you create a public/private key set on your development computer that you give to Apple. This process links the Apple Certificate Authority and you \(or your team\) in the creation of your signing certificate.

Then for each application or app extension, you create an App ID. The App ID links your signing certificate with your app \(identified by its bundle ID\).

Finally, you create one or more Provision Profiles. Each profile declares links to one of your App IDs and declares whether a particular build is destined for internal development, Ad Hoc distribution, or release distribution through the store. For internal development and Ad Hoc distribution, it may also contain the list of devices where the app may be tested or distributed.

That’s all the major players in the provisioning drama. But the devil is in the details and now we need to look at each piece of the provisioning puzzle in greater detail.

{% embed url="https://abhimuralidharan.medium.com/what-is-a-provisioning-profile-in-ios-77987a7c54c2" %}

{% embed url="https://blog.codemagic.io/how-to-code-sign-publish-ios-apps/" %}

{% embed url="https://blog.codemagic.io/distributing-native-ios-sdk-with-flutter-module-using-codemagic/" %}

{% embed url="https://www.bounteous.com/insights/2018/08/08/demystifying-ios-app-provisioning-process/?lang=en-ca" %}

{% embed url="https://www.codementor.io/@nitingohel/apple-developer-distribution-certificate-du107wwff" %}



Bitrise/ CI preferably uses manual code signing

Developing in xcode does not need certificates  
Connecting to device to test need `develop` cert at least  
Publishing needs `distribution` cert  


