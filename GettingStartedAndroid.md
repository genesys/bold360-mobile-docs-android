## System Requirements, Setup, and Installation Dependencies

## System Requirements  

* Java 8 or higher.
* Gradle 5.1.1 or higher.
* Android API 19 or higher.
* Android Studio.

## Setup

In an existing android project, update the project build.gradle file with the following: 

1. #### Add SDK dependencies
    [Check here for latest SDK version](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_Android_RN.html) 

    For example:
    ```gradle
    implementation "com.bold360ai-sdk.conversation:ui:[VERSION_NUMBER]"
    implementation "com.bold360ai-sdk.conversation:chatintegration:[VERSION_NUMBER]"
    implementation "com.bold360ai-sdk.conversation:engine:[VERSION_NUMBER]"
    implementation "com.bold360ai-sdk.core:sdkcore:[VERSION_NUMBER]"
    implementation "com.bold360ai-sdk.core:accessibility:[VERSION_NUMBER]"
    ```

2. #### Add the following to the `android{...}` block definition:
    ```gradle
    android {
        
        kotlinOptions {
            freeCompilerArgs = ['-Xjvm-default=compatibility']
            jvmTarget = "1.8"
        }

        //overcome the following error: "More than one file was found with OS independent path..."
        packagingOptions{
            pickFirst 'META-INF/atomicfu.kotlin_module'
            
            // those may also be needed:
            pickFirst 'META-INF/main_debug.kotlin_module'
            pickFirst 'META-INF/main_release.kotlin_module'
        }
    }
    ```
