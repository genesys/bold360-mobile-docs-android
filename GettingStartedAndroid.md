# System Requirements, Setup, and Installation Dependencies

## System Requirements  

* Java 8 or higher.
* Gradle 5.1.1 or higher.
* Android API 16 or higher. <sub>**(since version 3.5.0**)</sub>
* Android Studio.

## Setup

Apply the following to your app's build.gradle file: 

1. ### Add SDK dependencies
    [Check here for latest SDK version and needed dependencies](https://developer.bold360.com/help/EN/Bold360API/Bold360API/ReleaseNotesAndroid.html) 

    

2. ### Add the following to the _`android{...}`_ block definition:
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
