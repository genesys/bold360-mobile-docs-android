---
layout: default
title: Quick Start
nav_order: 2
---

# Quick start
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Use this Quick Start guide to get you up and running with the knowledge of fast integrating chat to your application.

## System Requirements  
{: .d-inline-block }

* Java 8 or higher.
* Gradle 5.3.6 or higher.
* Android Studio.
* The SDK supports devices with API level 16 or higher. <sub>**(since version 3.5.0**)</sub>


## Set up the SDK on your App.
{: .d-inline-block }

1. ### Import SDK dependencies
    {: .no_toc }
    Import the SDKs dependencies.
    [Check here for latest SDK version and needed dependencies](https://developer.bold360.com/help/EN/Bold360API/Bold360API/ReleaseNotesAndroid.html) 

    
2. ### Extra configurations on build.gradle:
    {: .no_toc }
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

---
### Now you are ready to integrate and create some chats.
{: .no_toc }

## Create and start a chat  
{: .d-inline-block }

Follow the next steps to create and start a chat.

1. ### Create an Account

    - [Create `BotAccount` for chat with AI](/docs/chat-configuration/setting_account#botaccount)
    {: .no_toc }
    
    - [Create `BoldAccount` for live chat with agent](/docs/chat-configuration/setting_account#boldaccount)
    {: .no_toc }
        
    - [Create `AsyncAccount` to start messaging](/docs/chat-configuration/setting_account#asyncaccount)
    {: .no_toc }  


2. ### Create ChatController
    With the ChatController one can create and control multiple chats.
    The chat type is configured by the Account that is provided on chat creation.

    ```kotlin
    val chatController = ChatController.Builder(context)
                                          .build(account, ...)

    ...
    // start a new chat, using same chatController:
    chatController.startChat(account)

    // restore active chat or starts new chat
    chatController.restoreChat(fragment?, account?)
    ```


3. ### Add the chat fragment to your activity.

    Implement the ChatLoadedListener interface and pass it in the `ChatController.Builder` build method.   
    Once the chat build succeeded and the fragment is ready to be displayed, `onComplete` will be called with the fragment on the result. 

    ```kotlin
    ChatController.Builder(context).build(account, object : ChatLoadedListener {
            override fun onComplete(result: ChatLoadResponse) {
                result.error?.run{
                  // report Chat load failed
                } ?: run{
                  // add result.getFragment() to the applications activity.
                }
            }
        })
    ```

---

##### **Notice** 
{: .no_toc } 
> Make sure to activate `ChatController.destruct()`, when the ChatController instance is no longer needed (exp: Chats section in app
  is being closed or the ChatController instance is being replaced). Destruct will verify resources release (including open sockets and communication channels).
    

---

### Code Sample
{: .no_toc .text-delta}
[bold360ai samples](https://github.com/bold360ai/bold360-mobile-samples-android)
