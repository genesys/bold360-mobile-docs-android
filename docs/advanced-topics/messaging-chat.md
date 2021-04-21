---
layout: default
title: Messaging Chat
parent: Advanced Topics
nav_order: 5

---

# Messaging Chat 
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
Messaging chat enables both the User and the Agent to connect at their own time to the chat and post their messages. There is no need for both parties to be connected at the same time for the entire chat.   
{: .overview}
The SDK provides the ability to create and reconnect to messaging conversations and sessions.   
Messaging chat configurations are provided by the hosting App over `AsyncAccount` instance, passed to the ChatConltroller on chat creation.
{: .overview}

--- 

## AsyncAccount
Use this account to create messaging chat sessions.   
On the account session configuration you can provide user details, configure to continue a previous chat, and more. 

### Create account
In order to create messaging chat, a valid apiKey and applicationId are required.

```kotlin
val account = AsyncAccount(API_KEY, APPLICATION_ID)
```

### Configure session
- **applicationId <sub>[mandatory]</sub>** - An id that was created by bold administration to your account. The applicationId can be provided on account creation or later set on the session info property.  
`account.info.applicationId = APPLICATION_ID`

- **userInfo <sub>[optional]</sub>** - Defines the user that this chat relates to. 
Messaging `Conversation` is a collection of chat sessions which relates to the same user, by userId.
`UserInfo` defines chat sessions relation to a user and provides some user details to be viewed by the chat accepting agent.   

  > By default, the account is created with a new UserInfo and an automatically generated userId.

  * <a id="relate"></a> **In order to relate chat sessions to a specific user**, set a UserInfo object with a specific userId value, to the account session configuration.   
  `account.info.userInfo = UserInfo(USER_ID)`

  * **In order to provide user details**, such as first/last name, email etc, you can provide them on your created UserInfo object or access the default created one.

    ```kotlin
    account.info.userInfo = UserInfo(USER_ID).apply{
        firstName = FIRST_NAME // optional
        lastName = LAST_NAME // optional
        email = EMAIL_ADDRESS // optional
        phoneNumber = PHONE_NUMBER // optional
    }
    // OR
    account.info.userInfo.apply{
        firstName = "..."
        email = "..." 
        ...  
    }
    ```

- **SenderId <sub>[optional]</sub>** - Chat session id. Should be used to fetch messages that were sent to the user from the agent, on that chat session, while the user was off.   If none provided, a new id will be created for the chat session. A new chat window will be opened on the agent console. 

- **ShouldStartNewChat <sub>[optional]</sub>** - Defines if a previous chat window, on agent workspace, should be used, for this session, if still open.

- **LastReceivedMessageId <sub>[optional]</sub>** - Defines the last message that was viewed by the user, in order to fetch missed messages from this point on. If not provided, will fetch messages from session start.

---

## Start a messaging chat
{: .d-inline-block .mt-2}   

The SDK provides the option to start/continue a messaging chat, or escalate to one from a chat with AI.

### Start/continue a messaging chat
- Create a [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) with an AsyncAccount.

    ```kotlin
    val account = AsyncAccount(API_KEY, APPLICATION_ID)
    val chatController = ChatController.Builder(context)                                                     
                                        .build(account, ...)
    ```
    
- Use an existing `ChatController` and call [`chatController.startChat`]({{'/docs/chat-configuration/extra/chatcontroller#start-chats' | relative_url}})/`chatController.restoreChat` with an AsyncAccount.
    ```kotlin
    val account = AsyncAccount(API_KEY, APPLICATION_ID)
    chatController.startChat(account)
    // or
    chatController.restoreChat(account = account)
    ```

### Escalate to a messaging chat from chat with AI
Chat escalation is done when the user selects a [Chat typed channel](https://support.nanorep.com/API-Integrations/Chat-Integration/1009694282/How-to-integrate-LiveChat-Inc-chat.htm) configured on the Bold360ai console, from a bot response options.   
The channel must contain the accounts apiKey and the applicationId as follows: 
> `async:APPLICATION_ID:API_KEY`

In case the hosting App implements the AccountInfoProvider, the [`AccountInfoProvider.provide`]({{'/docs/chat-configuration/extra/account-info-provider#account-provide' | relative_url}}) method will be called with a basic AsyncAccount, according to the data provided from the channel. The `provide` method call, enables the hosting App to [configure](#configure-session) what ever needed for the chat session.   
> If `AccountInfoProvider` implementation was not provided, a new chat conversation will be created with an atomated user details.

--- 

## Messaging chat continuity
The SDK provides the ability to connect to an open or closed chat session and fetch the messages that were sent to him while he was not actively connected to that chat.
The way the chat should start is configured by the AsyncAccount instance.   

To be able to connect to a user conversation and fetch messages that were sent to him while he was off, and be able to continue chatting with the agent if needed.
 
### How to configure
{: .mb-4}   
UserInfo   
{: .strong-sub-title}   
Provide a `UserInfo` with the relevant userId, of which the starting session should be added to his Conversation.
> If UserInfo (userId) was not provided on the account SessionInfo, a new UserInfo with auto generated userId will be created on chat start. this will start a new Conversation.

```kotlin
// sets a userInfo to the async session
asyncAccount.info.UserInfo = UserInfo(UserId).apply{ ... }
```

ShouldStartNewChat   
{: .strong-sub-title .mt-6}   

Defines whether the upcoming chat session should be restored from a previously opened session or a new session should be created.
```kotlin
asyncAccount.info.ShouldStartNewChat = true/false
```

SenderId   
{: .strong-sub-title .mt-6}   
SenderId of a previous created session. In case the session was not ended, by neither side, the same chat window will be used on the agent console, unless [`ShouldStartNewChat`](#shouldStartNewChat) value was configured to true. 
```kotlin
asyncAccount.info.SenderId = Long_Sender_Value
``` 
Provide a _SenderId_ in order to:
- Restore an **_active_** chat session. [`ShouldStartNewChat==false`]
    > Chat restore will fail if the chat session was already been closed by agent.    
    A new chat session will be created, with newly generated SenderId. Missed messages of the closed session will be fetched and displayed.
- Enable retrieval of missed messages from a previously opened/closed chat. [`ShouldStartNewChat==true`]  
   
LastReceivedMessageId   
{: .strong-sub-title .mt-6}   
In order to get only the messages that the user did not yet received, configure [`LastReceivedMessageId`](#lastReceivedMessageId), occording to the last message id, provided on the [session updates](#how-to-get-lastReceivedMessageId-updates).
     
```kotlin
asyncAccount.info.LastReceivedMessageId = Last_Message_Id
``` 
  
---

## Account updates
- Once the chat session was created, an account update will be triggered on your [`AccountInfoProvider`]({{'/docs/chat-configuration/setting-account/account-info-provider' | relative_url}}) implementation.  
From the updated account you can fetch the `SenderId`, and be synced with the last created session id.
`account.info.SenderId` 

- In order to get updates for `LastReceivedMessageId`, implement [`AccountSessionListener`]({{'/docs/chat-configuration/setting-account/account-info-provider#ongoing-session-configurations-changes' | relative_url}}) instead. 

---

## How to
- ### Start a chat session that will [relate to the same user](#relate)
    
- ### Get messages that were sent while the user was off.
    1. [Configure the session UserInfo](#configure-user-info) with the specific user id. 
    2. Configure the `SenderId` with the id of the session that the messages were sent on.
    3. Configure `LastReceivedMessageId` with the last agent message that was viewed by the user, in order to get only the relevant messages.
    The users missed messages will be fetched from the previous session, if has any.

    ```kotlin
    account.info.apply{
        account.info.userInfo = UserInfo(USER_ID)
        SenderId = PREVIOUS_SESSION_ID
        LastReceivedMessageId = LAST_AGENT_MESSAGE_ID
    }
    ```

- ### Continue chat on an open/new chat window
    1. Configure `UserInfo` and `SenderId` and `LastReceivedMessageId` as mentioned before.
    2. Configure `ShouldStartNewChat` to `false` to use an open chat window otherwise set to `true`     
    
    If the previous session chat was closed by the user or the agent, a new chat window will be created with a new SenderId, no matter what was configured on `ShouldStartNewChat`. The users missed messages will be fetched from the previous session.

    ```kotlin
    account.info.apply{
        account.info.userInfo = UserInfo(USER_ID)
        SenderId = PREVIOUS_SESSION_ID
        LastReceivedMessageId = LAST_AGENT_MESSAGE_ID
        ShouldStartNewChat = false/true
    }
    ```