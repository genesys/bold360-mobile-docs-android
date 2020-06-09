# Voice to Voice

### How to enable
A `ConversationSettings` object with overriding configurations, that define the chat behavior and features enabling status can be set on the ChatController builder. one of it's method enables to define the <U>Voice support level</U> that is needed for the chat.

#### Voice support level:  
- **VoiceSupport.None** - speech recognition is disabled.  
- **VoiceSupport.SpeechRecognition** - only speech recording is enabled. you can record your message and send it to the other chat partner.  
- **VoiceSupport.VoiceToVoice** - enables both speech recognition and response read out by the device.  
- **VoiceSupport.handsFree** - voice to voice with auto listening and send of user messages and read out of responses.

Voice support feature can be configured on `ConversationSettings` as follows:
```kotlin
val settings = ConversationSettings().voiceSettings(
                    VoiceSettings(VoiceSupport.VoiceToVoice))

val chatController = ChatController.Builder(context)
            .conversationSettings(settings)
            .build(account, object : ChatLoadedListener {...}
```
> **Notice:** The voice settings you provide on ConversationSettings are definition of what you expect to be available. But the actual support of voice configuration is determine also by the chat type support.   
_Currently we support VoiceToVoice on ai chats and SpeechRecognition on live chats._


### Provide an alternative text for the read out
When voice support feature is configured to `Voice to Voice`, Once a response is received, that response will be parsed to a textual presentation to be read out.   
All parts of the response are combined and read. message body, persistent options and quick options, if available. Each read section has a configurable prefix.   
In case there's a need to alternate some of the `read out` text, before it is read to the user, implement and set a `TTSReadAlterProvider`to the ChatController instance.
> The default text to be read, as provided by the SDK implementation, is available on `readRequest.readResult` property. this text can be changed, 
or the response details provided on `readRequest.readoutMessage` can be used to create a different readoutResult outcome.
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

// 2. pass to the ChatController:
// on ChatController build:
 val chatController = ChatController.Builder(context)
            .ttsReadAlterProvider(ttsProvider)
            .build(account, object : ChatLoadedListener {...}

// OR on runtime:
chatController.setTTSReadAlterProvider(provider)

// Passing null, will clear the alter provider. Default readout provider will be used to prepare responses to be read.

```


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
