## Chat continuation
The option to restore and continue a previously created chat later in time.    

#### Chat SessionInfo
Each chat type (bot, live, Messaging) has its own properties that are being used to enable continuity.   
Those properties are configurable on the accounts `SessionInfo` property.   

##### Listening to session data updates
While the chat is in progress, session data may change.  
In order to be updated with such changes for later use, implement [_AccountInfoProvider_ or _AccountSessionListener_](android-AccountInfoProvider) and pass implementation on `ChatController` creation. 
```kotlin
val chatController = ChatController.Builder(context).apply{
                        accountProvider(MyProvider)
                     }               
                    .build(account, ...)
```


#### In this topic:
- [Messaging chat continuation](AsyncChatContinuation)
- [Bold live chat continuation](BoldChatContinuation)
- [Bot ai chat continuation](BotChatContinuation)
