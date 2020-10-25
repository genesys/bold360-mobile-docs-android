---
layout: default
title: AsyncAccount
parent: Chat Account
grand_parent: Chat Configuration
nav_order: 3
# permalink: /docs/chat-configuration/chat-account/async-account
---

# AsyncAccount
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Use this account to create messaging chat sessions.
```kotlin
val account = AsyncAccount(API_KEY, APPLICATION_ID)
```

## Configure chat session
In order to create messaging chat, a valid apiKey and applicationId are required.

- **applicationId <sub>[mandatory]</sub>** - An id that was created by bold administration to your account. The applicationId can be provided on account creation or later set on the session info property.  
`account.info.applicationId = APPLICATION_ID`

- **userInfo <sub>[optional]</sub>** - Defines the user that this chat relates to. Uses for relating chat sessions to a specific user, and to provide some user details to be viewed by the chat accepting agent.   
  
  ### Configure user info
    By default, the account is created with a new UserInfo and an automatically generated userId.

    - <a id="relate"></a> **In order to relate chat sessions to a specific user**, set a UserInfo object with the specific user, userId value, to the account.
    `account.info.userInfo = UserInfo(USER_ID)`

    - **In order to provide user details**, such as first/last name, email etc, you can provide them on your created UserInfo object or access the default created one.

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

- **SenderId <sub>[optional]</sub>** - Chat session id. Should be used to fetch messages that were sent to the user from the agent, on that chat session, while the user was off.

- **ShouldStartNewChat <sub>[optional]</sub>** - Defines if a previous chat window, on agent workspace, should be used, for this session, if still open.

- **LastReceivedMessageId <sub>[optional]</sub>** - Defines the last message that was viewed by the user, in order to fetch missed messages from this point on. If not provided, will fetch messages from session start.

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

---

## Account updates
- Once the chat session was created, an account update will be triggered on your [`AccountInfoProvider`](/docs/chat-configuration/setting-account/account-info-provider) implementation.  
From the updated account you can fetch the `SenderId`, and be synced with the last created sesion id.
`account.info.SenderId` 

- In order to get updates for `LastReceivedMessageId`, implement [`AccountSessionListener`](/docs/chat-configuration/setting-account/account-info-provider#ongoing-session-configurations-changes) instead. 