---
layout: default
title: Voice support - Android 11
parent: FAQ
nav_order: 4
permalink: /docs/faq/android-11-voice
---

# Voice support on Android 11+
{: .no_toc}

--- 

## Overview
Android 11 introduces changes on how the App can query and interact with other installed apps on the device.
In order to access installed services and apps, the App should specify those in need, using the `<queries>` tag on the Apps `AndroidManifest.xml` file.

`Speech recognition` and `Text to speech` capabilities are among those services that should be specified on the manifest under the `<queries>` tag.

❗ Without those configurations `Voice` support will not be available to the Bold360 SDK, on devices running Android 11 and above.

## How to enable voice support on Android 11
Follow those steps in order to enable Voice support in Bold360 SDK on Android 11 devices:

1) Add the following to the `AndroidManifest.xml` file.
{: .light-title}
```xml
<queries>
    <intent>
        <action android:name="android.speech.RecognitionService" />
    </intent>
    <intent>
        <action android:name="android.intent.action.TTS_SERVICE" />
    </intent>
</queries>
```

{: .mt-6}
2) Upgrade gradle plugin version
{: .light-title}
  Apps that are using gradle plugin <b>_lower than 4.1_</b>, should upgrade to the closest compatible version as defined here [Preparing your Gradle build for package visibility in Android 11](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html#android-gradle-plugin-fixes)
