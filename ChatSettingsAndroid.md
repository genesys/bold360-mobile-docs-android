# Conversation settings
Chat features are configured by the various consoles available for every chat type, and by SDK pre defined options.  
In order to intervene in those configurations from the app side, we provide you the option to pass your customed `ConversatioSettings` on chat creation.
```kotlin
val settings = ConversationSettings()
    // do some customed change

val chatController = ChatController.Builder(context).apply {
                        conversationSettings(settings)
                        ....
                     }.build(account...)
```


## Enable/Disable chat records request
In order to change and control which chat scope can request the preceding chat records, use the following:
```kotlin
val conversationSettings = ConversationSettings()
                            .enableChatLogRequest(Requesting_Scope, isEnabled)
```

> Exp: In order to enable passing preceding bot chat records to the live agent:
  ```kotlin
  ConversationSettings().enableChatLogRequest(StatementScope.BoldScope, true)         
  ```   
 
 - By default, `BoldScope` chat is enabled to request preceding chats. 