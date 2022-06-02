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

Use this Quick Start guide to get you up and running with a working AI or live chat hosted by your App.   

## System Requirements  

* Java 8 or higher.
* Gradle 5.3.6 or higher.
* Android Studio.
* The SDK supports devices with API level 16 or higher. <sub>**(since version 3.5.0**)</sub>

## Set up the SDK on your App.
{: .d-inline-block }

1. ### Import SDK dependencies 
    {: .no_toc .strong-sub-title}   
    
    [Check here for latest SDK version and needed dependencies]({{'/docs/release-notes#dependencies' | relative_url}}) 

    
2. ### Extra configurations on build.gradle:
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


    - If your app is configured with shrink and minify turned on, please read [Shrink and Obfuscattion support]({{'/docs/faq/shrink-and-obfuscate' | relative_url}})
    
---

#### 👍  Now you are ready to integrate and create some chats.
{: .no_toc .strong-sub-title}

---

## Create and start a chat  
{: .d-inline-block .mt-4}

Follow the next steps to create and start a chat.

1. ### Create an Account
    - [Create `BotAccount` for chat with AI]({{'/docs/chat-configuration/chat-account/bot-chat#botaccount' | relative_url}})
    {: .no_toc }
    
    - [Create `BoldAccount` for live chat with agent]({{'/docs/chat-configuration/chat-account/bold-chat#boldaccount' | relative_url}})
    {: .no_toc }
        
    - [Create `AsyncAccount` to start a messaging chat]({{'/docs/advanced-topics/messaging-chat#asyncaccount' | relative_url}})
    {: .no_toc }  
---

2. ### Create [ChatController]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}})
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
---

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
> Make sure to activate `ChatController.destruct()`, when the ChatController instance is no longer needed (e.g., Chats UI in app is being closed or the ChatController instance is being replaced). Destruct will verify resources release (including open sockets and communication channels).  

---

### Code Sample
{: .no_toc .text-delta}
[bold360ai samples](https://github.com/genesys/bold360-mobile-samples-android)
