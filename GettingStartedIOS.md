# Getting Started with iOS Devices

## System Requirements  

This article will help you get started with the development of bold360ai's SDK for iOS.

* [iOS 9 and above](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-SW1)
* Automatic Reference Counting (ARC) is required in your project.
* With the release of iOS 9 and Xcode 7, a new feature called [App Transport Security (ATS)](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) was introduced.
* [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).

## Setup

>Note: Using CocoaPods on an existing Xcode project will modify the project file, so you may want to make a backup before doing this.
1. Create a Podfile in the root directory of your project.

```sh
$ pod init
```

2. Add Official CocoaPods PodSpecs repository to your Podfile:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
```

3. Add bold360ai PodSpecs repository to your Podfile:

```ruby
source 'https://github.com/nanorepsdk/NRSDK-specs.git'
```

4. Add bold360ai iOS SDK to your Podfile:
    
```ruby
pod 'Bold360AI'
```

5. Using *terminal*, with your project root directory as the *working path*, run:

```ruby
pod install
# pod update (use this when you want to get new released version).
```

>This will download all the necessary files which are required to integrate the bold360ai into your project.