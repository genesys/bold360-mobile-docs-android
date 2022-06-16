---
layout: default
title: Voice
parent: Advanced Topics
nav_order: 5
# permalink: /docs/advanced-topics/voice
---

# Voice 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Voice support on Android 11]({{'/docs/faq/android-11-voice' | relative_url}})

---

## Overview
The SDK supports some voice related features, such as speech to text and text to speech.   
Those features activation is defined by support levels. Each level adds some voice capabilities.      
{: .overview}

{: .mt-6}
## Enabling Voice support on your application
In order to enable Voice support on your application, go through the following steps:
1. Add pemission 
    On the application manifest add the following permission.   
    `<uses-permission android:name="android.permission.RECORD_AUDIO"/>`

2. __None androidx applications__  
    {: .notice} 
    If the chat activity overrides `onRequestPermissionsResult` method, make sure to call it's super:   
    `super.onRequestPermissionsResult(requestCode, permissions, grantResults)`.

{: .mt-6}
## Configure voice support level
The level of support can be configured on the ChatController creation, by ConversationSettings object.   
Use `ConversationSettings.voiceSettings` to define the **desired** <U>Voice support level</U> for the chats that will be created by the `ChatController` instance.   
Actual voice support level is determine by the chat type support, where the configured level is the maximum allowed.

```kotlin
val settings = ConversationSettings().voiceSettings(
                    VoiceSettings(VoiceSupport.HandsFree))

val chatController = ChatController.Builder(context)
                        .conversationSettings(settings)
                        .build(account, object : ChatLoadedListener {...})
```

### The available voice support levels:  
{: .no_toc}
- **VoiceSupport.None** - speech recognition is disabled.  
- **VoiceSupport.SpeechRecognition** - only speech recording is enabled. you can record your message and send it to the other chat partner.  
- **VoiceSupport.VoiceToVoice** - enables both speech recognition and response read out by the device.  
- **VoiceSupport.HandsFree** - voice to voice with auto listening and send of user messages and read out of responses.

> ⚜️ _AI chats can be configured with level up to `HandsFree`, and live chats up to `SpeechRecognition`._

❗ **It is not recommanded to set support level to `HandsFree` mode while [accessibility]({{'/docs/faq/accessibility' | relative_url}}) service is turned on, on the end user device. The two works with the same device's speak resources, which will cause SDK's auto read and recording of messages to malfunction.**
{: .mt-6}

---

## Voice configurations
- ### Configure spoken voice
    The configurations relevant to the read out voice; language, speech rate, volume, pitch, etc, are available via the ChatController. Those configurations can be changed at any time.
    ```kotlin
    chatController.configuration.ttsConfig.apply {
                        language = Locale.FRENCH // defaults to default locale
                        volume = 0.5f // defaults to 1.0f which is max volume
                    }
    ```
    > 👉 The language you set to the TTSConfig should be supported by the device, otherwise, the default locale will be used.
  

- ### Configure voice related UI components
    {: .mt-6}
    Some UI configurations are available to be changed via `SendCmpUIProvider::uiConfig`
    ```kotlin
    val chatUIProvider = ChatUIProvider().let{
        it.chatInputUIProvider.sendCmpUIProvider.uiConfig.apply{
            //... change default configurations here
            sendImage = ...

            speecherUIConfig.apply{
                //... change default voice related UI configurations here
                speakerImage = ...
                micImage = ...
            }
        }
    }

    // apply custom UI provider to the ChatController
    val chatController = ChatController.Builder(context)
                .chatUIProvider(chatUIProvider)
                .build(account, object : ChatLoadedListener {...}
    ```


## Alternative readout
Provide an alternative text for the read out
When voice support level is configured to `VoiceSupport.VoiceToVoice` or `VoiceSupport.HandsFree`, every voice sourced response will be parsed to a textual format to be read out.   

📚 **Voice sourced response** = Response to a query that was fully or partially recorded by the user.

The default readout implementation combines the response body and available options to one readable string.

The SDK enables the hosting App to alternate some or all of the response readable text.
In order to do so, an implementation of `TTSReadAlterProvider` should be set on the `ChatController` instance.   
Once a readout response is received the `TTSReadAlterProvider.alter` method will be called.    

📚 The response readout details are provided by `ReadRequest.readoutMessage`.   

### Custom alter provider
Implement a custom alter provider to change the result message for readout.  

Custom implementation can change the result message as follows:   
1. Change the message details on `readRequest.readoutMessage`, and by that create an alternative string representation.
2. Change the result text on `readRequest.readResult`.  

📚  **The final desired result string should be set on `readRequest.readResult`.**  

Once the custom provider done with message alternation, the provided callback should be activated (providing as parameter the **ReadRequest**).

```kotlin
// 👉 1. implement the provider:
val ttsProvider = object : TTSReadAlterProvider{
    override fun alter(readRequest: ReadRequest, 
                        callback: (ReadRequest) -> Unit) {
                            
        //>> Option 1 - override readoutMessage details to get a customed result: 
        readRequest.readoutMessage.apply {
            // Persistent options Specific prefix override:
            persistentOptions?.forEachIndexed { index, option ->
                option.prefix = "Persistent option no. ${index + 1}"
            }

            // Quick options Specific prefix override :
            quickOptions?.forEachIndexed { index, option ->
                option.prefix = "Quick option no. ${index + 1}"
            }

            // Persistent options General prefix override:
            setPersistentPrefix("Persistent Options")

            // Quick options General prefix override :
            setQuickPrefix("Quick Options")

            // For General prefix for carousel options:
            (this as? CarouselReadoutMessage)?.setPrefixToItemsOptions("Carousel Option");            
            // Message body override
            body = "Altered message Body"
        }
        ///////////////////////////////////
                            
        //// Option 2: Override the provided default readout result:
        readRequest.readoutResult = "..." // set prepared string to be read 

        ///////////////////////////////////
        
        callback(readRequest) // pass back your changes
    }
}

// 👉 2. Set your alter provider:
//>> option 1: On ChatController build:
val chatController = ChatController.Builder(context)
            .ttsReadAlterProvider(ttsProvider)
            .build(account, object : ChatLoadedListener {...}

//>> Option 2: On runtime:
chatController.setTTSReadAlterProvider(provider)
```

📚 Setting the TTSReadAlterProvider to null, releases the alter provider reference. Default readout provider will be used to prepare responses readout text.

## How to stop a readout 
If a message readout should be stopped, e.g. when there's an incoming phone call or other interruptions, use `chatController.onChatInterruption()` API, to stop the current active readout.

```kotlin
// Listening to incoming calls, in order to stop readout in progress:
LocalBroadcastManager.getInstance(baseContext).registerReceiver(
    object : BroadcastReceiver() {
        override fun onReceive(context: Context, intent: Intent) {
            Log.d("callAction", "Got broadcast on call action")
            if (chatController.isActive) {
                chatController.onChatInterruption()
            }
        }
    }, IntentFilter("android.CHAT_CALL_ACTION")
)
```