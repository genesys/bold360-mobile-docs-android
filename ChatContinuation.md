## Chat continuation
The option to restore and continue previously created chat later in time.  
Each chat type (bot, live) has its own properties that are being used to enable that option.   
Those properties are configurable via the chat account.  

#### Session data updates
In order to get updates of chat session data, implement [`AccountInfoProvider`](./android-AccountInfoProvider) and pass it on `ChatController` creation. 
```kotlin
val chatController = ChatController.Builder(context).apply{
                        accountProvider(MyProvider)
                     }               
                    .build(account, ...)
```

#### In this topic:
- [Async chat continuation](AsyncChatContinuation)
- [Bold live chat continuation](AsyncChatContinuation)
- [Bot ai chat continuation](AsyncChatContinuation)