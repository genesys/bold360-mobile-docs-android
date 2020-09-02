## Messaging conversation
Messaging chat enables both the User and the Agent to connect at times to the chat and post their messages. There is no need for both parties to be connected at the same time for the entire chat.   

Messaging `Conversation` is a collection of chat sessions that are connected by one UserId.    

### Messaging chat session in mobile SDK
The SDK provides the ability to create and reconnect to conversations and sessions.
Messaging chat configurations are provided by the embedding App over an `AsyncAccount` instance passed to the ChatConltroller on chat creation.

#### Create Messaging Conversation
All you need is to create an AsyncAccount with 2 required keys:
   - Apikey - Customer provided key created by Bold admin.
   - ApplicationId - Customer provided id created by Bold admin.
```kotlin
val account = AsyncAccount(APIKey, ApplicationId)
val chatController = ChatController.Builder(context) 
                        ...
                        .build(account, ...)
```
That's it. 

But in order to add this chat to a User's conversation collection, the userId passed over the account should match that userId.
```kotlin
val account = AsyncAccount(APIKey, ApplicationId).apply{
    info.UserInfo = UserInfo(userId)
}
```
Each time this account will be used, a new session will be created, and be added to the same user chat history.

#### Passing user details
User can have some defining details accept for his id, which can be passed to the agent side.
User details can be provided on the AsyncAccount.
```kotlin
val account = AsyncAccount(APIKey, ApplicationId).apply{
    info.UserInfo = UserInfo(userId).apply{
        firstName = //
        lastName = //
        email = //
        ...
    }
}
```

### Messaging chat continuation
The SDK provides the ability to connect to an open or closed chat session and fetch the messages that were sent to him while he was not actively connected to that chat.
The way the chat should start is configured by the AsyncAccount instance.   
see [Messaging chat continuity](./AsyncChatContinuation.md) for more details.

   
 