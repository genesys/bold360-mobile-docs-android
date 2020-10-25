---
layout: default
title: Voice
parent: Advanced Topics
nav_order: 6
# permalink: /docs/advanced-topics/voice
---

# Voice {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
...

## Configure availability
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
- **VoiceSupport.None** - speech recognition is disabled.  
- **VoiceSupport.SpeechRecognition** - only speech recording is enabled. you can record your message and send it to the other chat partner.  
- **VoiceSupport.VoiceToVoice** - enables both speech recognition and response read out by the device.  
- **VoiceSupport.HandsFree** - voice to voice with auto listening and send of user messages and read out of responses.

> **Notice:** _Currently we support HandsFree on ai chats and SpeechRecognition on live chats._

---

## How to configure
- ### Configure spoken voice
    The configurations relevant to the read out voice; language, speech rate, volume, pitch, etc, are available via the ChatController. Those configurations can be changed at any time.
    ```kotlin
    chatController.configuration.ttsConfig.apply {
                        language = Locale.FRENCH // defaults to default locale
                        volume = 0.5f // defaults to 1.0f which is max volume
                    }
    ```
    > Notice: The language you set to the TTSConfig should be supported by the device, otherwise, it will use the default locale.

- ### Configure voice related UI components
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

    // apply customed UI provider to the ChatController
    val chatController = ChatController.Builder(context)
                .chatUIProvider(chatUIProvider)
                .build(account, object : ChatLoadedListener {...}
    ```


## Alternative readout
Provide an alternative text for the read out
When voice support level is configured to `VoiceSupport.VoiceToVoice` or `VoiceSupport.HandsFree`, every voice sourced response will be parsed to a textual format to be read out.   
> * **Voice sourced response** = Response to a query that was fully or partially recorded by the user.

The default readout implementation combines the response body and available options to one readable string.

The SDK enables the embedding App alternate some or all of the response text that will be read to the user, before the actual read.   
An implementation of `TTSReadAlterProvider` should be set on the `ChatController` instance.   
Once a readout response is received the `TTSReadAlterProvider.alter` method will be called.    

The response readout details are provided by `ReadRequest.readoutMessage`.   

### The customed alter provider implementation can change the result message by:   
1. Change the message details on `readRequest.readoutMessage`, and by that create an alternative string representation.
2. Change the result text on `readRequest.readResult`.  

> **The final desired result string should be set on `readRequest.readResult`.**  

<u>As last action, the customed provider should call the provided callback with the alternated **ReadRequest**.</u>

```kotlin
// 1. implement the provider:
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

// 2. Set your alter provider:
//>> option 1: On ChatController build:
val chatController = ChatController.Builder(context)
            .ttsReadAlterProvider(ttsProvider)
            .build(account, object : ChatLoadedListener {...}

//>> Option 2: On runtime:
chatController.setTTSReadAlterProvider(provider)
```
> Setting the TTSReadAlterProvider to null, releases the alter provider reference. Default readout provider will be used to prepare responses readout text.
