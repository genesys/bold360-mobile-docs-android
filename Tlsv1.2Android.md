## TLSv1.2 
Transport Layer Security protocol.

Android devices can be in one of the following states:
- Device does not have TLS 1.2 installed at all.
- Device has TLS 1.2 installed, but not enabled by default.
- Device has TLS 1.2 installed and enabled by default. 

Since Bold systems are now under this security protocol, The Bold SDK as well was updated to support it.   
The Bold SDK enables the TLSv1.2 protocol on lower API (< 21) devices which are usually doesn't enable this option by default, even if installed.

### Embedding App responsibility
**It is up to the embedding App to verify and suggest installation of this protocol when was not installed before.**  
<u>Google play services</u> library, provides an easy way to do that:   
Using `ProviderInstaller.installIfNeeded` and `ProviderInstaller.installIfNeededAsync`   
> - [Check out here for more info on how to install](https://developer.android.com/training/articles/security-gms-provider)   
> - [Watch on our samples](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/MainActivity.kt)

**<u>Notice</u>**
> - If your App uses android **Support Library** - make sure you import the appropriate Google play version: `com.google.android.gms:play-services-base:16.1.0`
> - If your App uses **AndroidX** libraries, import the latest version.

