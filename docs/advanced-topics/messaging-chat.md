---
layout: default
title: Messaging Chat
parent: Advanced Topics
nav_order: 5
permalink: /docs/advanced-topics/messaging-chat
---

# Messaging Chat {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

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

   
 
## Messaging chat continuation
The SDK provides the ability to connect to an open or closed chat session and fetch the messages that were sent to him while he was not actively connected to that chat.
The way the chat should start is configured by the AsyncAccount instance.   

To be able to connect to a user conversation and fetch messages that were sent to him while he was off, and be able to continue chatting with the agent if needed.

### What is a Messaging conversation
Messaging conversation is a collection of chat sessions that are connected by UserId.   
**Messaging chat session can be:**
1. <u>Created</u> - new SenderId will be generated and a new chat window will be opened on the agent console.
2. <u>Reconnected</u> - SenderId of a previous created session will be used. In case the session was not ended before by neither side, the same chat window will be used on the agent console.    
 
User "missed" messages can be fetched, no matter if a new session was started or a previous one was reconnected, as long as the embedding App provides the SenderId of the session that was active while the messages were sent.
In order to get only the messages that the user did not yet received, the embedding App should provide the `LastReceivedMessageId`, the last message id which the user got from the agent.

#### How to get LastReceivedMessageId updates 
see [AccountSessionListener](./android-AccountInfoProvider.md) for details

### What is needed to enable chat continuation: 
- #### UserInfo - 
  Provide a `UserInfo` with the relevant userId, of which the starting session should be added to his Conversation.
  > If UserInfo (userId) was not provided on the account SessionInfo, a new UserInfo with auto generated userId will be created on chat start. this will start a new Conversation. 
  ```kotlin
  // sets a userInfo to the async session
  asyncAccount.info.UserInfo = UserInfo(UserId).apply{ ... }
  ```

- #### ShouldStartNewChat -
  Defines whether the upcoming chat session should be restored from a previously opened session or a new session should be created.
  ```kotlin
  asyncAccount.info.ShouldStartNewChat = true/false
  ```

- #### SenderId - 
  SenderId represents a chat session id. User can have many chat sessions in which accumulate to a user conversation.   
  SenderId functionality depends on [`ShouldStartNewChat`](#shouldStartNewChat) value. 
  ```kotlin
  asyncAccount.info.SenderId = Long_Sender_Value
  ``` 
  Provide a _SenderId_ in order to:
   - Restore an **_active_** chat session. [`ShouldStartNewChat==false`]
     > Chat restore will fail if the chat session was already been closed by agent.    
     A new chat session will be created, with newly generated SenderId. Missed messages of the closed session will be fetched and displayed.
   - Enable retrieval of missed messages from a previously opened/closed chat. [`ShouldStartNewChat==true`]  
     >For that purpose, you'll also need to provide the [`LastReceivedMessageId`](#how-to-get-lastReceivedMessageId-updates) 

     ```kotlin
     asyncAccount.info.LastReceivedMessageId = Last_Message_Id
     ``` 
  
### How to get session data updates:

When chat is being created, or when values were changed, an update call will be triggered over [`AccountInfoProvider`](/android-AccountInfoProvider.md).   
Save session details for later use.
