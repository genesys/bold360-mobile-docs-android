---
layout: default
title: Shrink and Obfuscate 
parent: FAQ
nav_order: 5
permalink: /docs/faq/shrink-and-obfuscate
---

# Shrink and Obfuscation support
{: no_toc}

---

## Overview
App build may be configured to enable shrinking and obfuscation in order to optimize and reduce apk size and remove unused code and resources.   
Sometimes there's a need to add some clarifications/rules to the obfuscation mechanism to preserve some classes, interfaces, etc, that may be removed otherwise, causing the app to become nonfunctional and crashing.  
Rules and other optimization configurations are set on the apps `proguard-rules.pro` file. 
{: .overview}

---

In order to enable the Bold SDK to work properly while shrinking and obfuscation options turned on, add the following rules.
{: .strong-sub-title}

```groovy
-keepattributes *Annotation*

# Preserves the SDKs API model classes
-keep class com.nanorep.nanoengine.model.** {*;}

# Preserves the SDKs chat model classes
-keep class com.nanorep.convesationui.structure.elements.**{*;}

-keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
}

# Preserve the special static methods that are required in all enumeration classes.
-keepclassmembers,allowoptimization enum * {
    <fields>;
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

# Prevent proguard from stripping interface information from TypeAdapter, TypeAdapterFactory,
# JsonSerializer, JsonDeserializer instances (so they can be used in @JsonAdapter)
-keep class * implements com.google.gson.TypeAdapter
-keep class * implements com.google.gson.InstanceCreator
-keep class * implements com.google.gson.TypeAdapterFactory
-keep class * implements com.google.gson.JsonSerializer
-keep class * implements com.google.gson.JsonDeserializer

# Prevent R8 from leaving Data object members always null
-keepclassmembers class * {
  @com.google.gson.annotations.SerializedName <fields>;
}

-keep class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator *;
}
```