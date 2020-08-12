# Voice to Voice

### How to enable
Use `ConversationSettings.voiceSettings` to define the **desired** <U>Voice support level</U> for the chats that will be created by the ChatController instance.   
Actual voice support level is determine by the chat type support, where the configured level is the maximum allowed.

```kotlin
val settings = ConversationSettings().voiceSettings(
                    VoiceSettings(VoiceSupport.HandsFreeVoiceToVoice))

val chatController = ChatController.Builder(context)
                        .conversationSettings(settings)
                        .build(account, object : ChatLoadedListener {...})
```

#### The available voice support levels:  
- **VoiceSupport.None** - speech recognition is disabled.  
- **VoiceSupport.SpeechRecognition** - only speech recording is enabled. you can record your message and send it to the other chat partner.  
- **VoiceSupport.VoiceToVoice** - enables both speech recognition and response read out by the device.  
- **VoiceSupport.HandsFreeVoiceToVoice** - voice to voice with auto listening and send of user messages and read out of responses.

> **Notice:** _Currently we support HandsFreeVoiceToVoice on ai chats and SpeechRecognition on live chats._


### Provide an alternative text for the read out
When voice support level is configured to `Voice to Voice`, every voice sourced response will be parsed to a textual presentation to be read out.   
> * **Voice sourced response** - Response to a query that was fully or partially recorded by the user.

Default readout implementation combines the response body and available options to one readable string.

The SDK enables the embedding App alternate some or all of the `read out` text, before it is read to the user.   
An implementation of `TTSReadAlterProvider` should be set on the ChatController instance.
Once a readout response is received the `TTSReadAlterProvider.alter` method will be called.   
Readout details are provided over `ReadRequest` instance.   
The App's implementation can go over the response details (`readRequest.readoutMessage`) and create an alternative string representation or just alter the default provided `readRequest.readResult`.  
**The final desired result should be set on `readRequest.readResult` property.**  
Once done, the provided callback should be called with the alternated ReadRequest.
```kotlin
// 1. implement the provider:
val ttsProvider = object : TTSReadAlterProvider{
    override fun alter(readRequest: ReadRequest, 
                        callback: (ReadRequest) -> Unit) {
                            
                            //// Option 1: 
      						// A. Override a specific content of the reaponse message details:
                            readRequest.readoutMessage.text = ... // override the body text here
      						readRequest.readoutMessage.setPersistentPrefix("new_prefix")
                            ...
      						// B. Activate the provided `toReadout()` method  
							readRequest.readoutResult = readRequest.readoutMessage.toReadout()
                            /////////
                            
                            //// Option 2: Override the provided default readout result:
                            readRequest.readoutResult = ... // alter text here

                            callback(readRequest) // pass back your changes
                        }
}

// 2. pass the provider to the ChatController:
// on ChatController build:
 val chatController = ChatController.Builder(context)
            .ttsReadAlterProvider(ttsProvider)
            .build(account, object : ChatLoadedListener {...}

// OR on runtime:
chatController.setTTSReadAlterProvider(provider)
```
> Setting the TTSReadAlterProvider to null, releases the alter provider reference. Default readout provider will be used to prepare responses readout text.

### How to configure
- #### Configuring read out settings
    The configurations relevant to the read out voice; language, speech rate, volume, pitch, etc, are available via the ChatController. Those configurations can be changed at any time.
    ```kotlin
    chatController.configuration.ttsConfig.apply {
                        language = Locale.FRENCH // defaults to default locale
                        volume = 0.5f // defaults to 1.0f which is max volume
                    }
    ```
    > Notice: The language you set to the TTSConfig should be supported by the device, otherwise, it will use the default locale.

- #### Configuring voice related UI components
    Some UI configurations are available to be changed via `SendCmpUIProvider::uiConfig`
    ```kotlin
    val chatUIProvider = ChatUIProvider().let{
        it.chatInputUIProvider.sendCmpUIProvider.uiConfig.apply{
            //... change default configurations here
            
            speecherUIConfig.apply{
                //... change default voice related UI configurations here
            }
        }
    }

    val chatController = ChatController.Builder(context)
                .chatUIProvider(chatUIProvider)
                .build(account, object : ChatLoadedListener {...}
    ```
