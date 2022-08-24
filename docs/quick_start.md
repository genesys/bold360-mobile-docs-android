---
layout: default
title: Quick Start
nav_order: 2
---

# Quick start
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

Use this Quick start guide to set up a working chat hosted by your App.

## System Requirements  

* Android Studio version 3.5.0 or higher
* Java 8 or higher
* Gradle 5.3.1 or higher, Android gradle plugin 4.0.0 or higher
* Kotlin plugin version 1.3.21 and higher
* Android 4.1 (API level 16) or higher


## Set up Bold SDK on your App.
{: .d-inline-block }

1. ### Configure gradle version
    {: .no_toc .strong-sub-title} 
    Open `File->Project Structure` on android studio top menu to configure the gradle version on your project. Set the version to at least the minimum required.
    > Gradle version and android gradle plugin version should be synced according to android development docs.
      In case of project sync failure or build error that relates to gradle, validate that the configured gradle versions are aligned.   
      [Gradle version matching](https://developer.android.com/studio/releases/gradle-plugin#updating-gradle)

2. ### Import SDK dependencies 
    {: .no_toc .strong-sub-title}   
    
   - Add the SDK's repository path to your project's/module's repositories configuration on the build.gradle file.   
     ```gradle
     repositories{
         ...
         maven { url "https://genesysdx.jfrog.io/artifactory/bold-android.prod/" }
     }
     ```
        
   - Add the following imports to your App's build.gradle file.

     ```gradle
     dependencies {
            ...
            implementation 'com.bold360ai-sdk.core:sdkcore:[Version_Number]'
            implementation 'com.bold360ai-sdk.conversation:chatintegration:[Version_Number]'
            implementation 'com.bold360ai-sdk.conversation:chatintegration:[Version_Number]'
            implementation 'com.bold360ai-sdk.conversation:ui:[Version_Number]'

            // optional:
            implementation 'com.bold360ai-sdk.core:accessibility:[Version_Number]'
     }
     ```

     [Check SDK's release notes for the latest available version]({{'/docs/release-notes#dependencies' | relative_url}}) 

    
3. ### Extra configurations on build.gradle:
    {: .no_toc .strong-sub-title}  
   
    Add the following to the _`android{...}`_ block definition

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
    {: .mb-6}


    - If your app is configured with the minifyEnabled set to true, please read [Shrink and Obfuscattion support]({{'/docs/faq/shrink-and-obfuscate' | relative_url}}) to make sure that the relevant rules are provided on the proguard file.
    
---

#### 👍  Now you are ready to integrate and create some chats.
{: .no_toc .strong-sub-title}

---

## Create and start a basic chat  
{: .d-inline-block .mt-4}

Follow the next steps to have a basic working chat integrated to your App.

1. Create and set the activity in which the chat will be displayed.

2. ### Create an Account
    - [Create `BotAccount` for chat with AI]({{'/docs/chat-configuration/chat-account/bot-chat#botaccount' | relative_url}})
    {: .no_toc }
    ```kotlin
    val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER)
    ```

    - [Create `BoldAccount` for live chat with agent]({{'/docs/chat-configuration/chat-account/bold-chat#boldaccount' | relative_url}})
    {: .no_toc }
    ```kotlin
    val account = BoldAccount(API_KEY).apply{
        addExtraData(...)
    }    
    ``` 
---

3. ### Create ChatController instance
    The [ChatController]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) provides APIs to create and manage chats.
    
    ```kotlin
    val chatController = ChatController.Builder(context)
                                          .build(account, ...)
    ...
    // start a new chat, using same chatController:
    chatController.startChat(account)

    // restore active chat or starts new chat
    chatController.restoreChat(fragment?, account?)
    ```
---

4. ### Display the chat on your activity

    If the ChatController build was successful, it creates a fragment on which the chat UI will be displayed. The created fragment should be added to your app's activity.

    To get the chat fragment, implement the `ChatLoadedListener` interface and pass the implementation to `ChatController.Builder` `build` method.   
    `ChatLoadedListener:onComplete` will be called with the chat creation outcome as a `ChatLoadResponse` instance.    
    `ChatLoadResponse` will contain the chat fragment if the chat was created properly.

    ```kotlin
    ChatController.Builder(context).build(account, object : ChatLoadedListener {
            override fun onComplete(result: ChatLoadResponse) {
                result.error?.let{
                  // report Chat load failed
                } ?: let{
                  // add result.getFragment() to the applications activity.
                  supportFragmentManager.beginTransaction().replace(R.id.chat_container, result.fragment, tag).commit()
                }
            }
        })
    ```

    > Verify that there is no existing chat fragment added to your activity if there is, make sure to remove it before adding the newly provided fragment.

---

5. ### Release chat resources on activity destroy
   Activate `ChatController.destruct()` when the [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) instance is no longer needed. Destruct will release referenced open resources.

---

### Code Sample
{: .no_toc .text-delta}
[bold360 samples](https://github.com/genesys/bold360-mobile-samples-android)
-
