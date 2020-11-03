---
layout: default
title: Bold Live Chat
parent: Chat Account
grand_parent: Chat Configuration
nav_order: 2
---

# Chat with live agent {{site.data.vars.force-work}}
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The SDK provides the tools to create and start a chat which will be answered by a live agent.
Both chat parties should be connected during the chat session.
{: .overview}

---

## BoldAccount
Use this account to create live chat sessions with an agent.

With the account you can configure and set chat and user details, set session parameters, like destind department, language usage, etc.

### Creating account

```kotlin
val account = BoldAccount(API_KEY)
```  
- API_KEY - As was created on the admin console.

### Configure chat session
Chat session and details can be configured by the account.   
User details can be provided for the chat session, and will be available for the receiving agent to view.     

- **extraData** - Detailes about the user and the current chat session. The `extraData` details will be used to fill the prechat form if enabled, and will provide the agent some details about the user.
```kotlin
account.addExtraData(VisitorDataKeys.FirstName to "Ando",
                            VisitorDataKeys.LastName to "Roid",
                            VisitorDataKeys.Email to "a@gmail.com",
                            ...)
```

- **securedInfo** - An encrypted secured string that was applied to the specific access key in order to validate the chat origin on creation.

- **VisitorId** - if provided, the created chat will be added to the same user account chats history. The same id will be used on the new chat.

```kotlin
account.apply{
    // adding extraData: 
    addExtraData (SessionInfoKeys.Department to BOLD_DEPARTMENT,
        SessionInfoKeys.FirstName to DemoFirstName,
        SessionInfoKeys.LastName to DemoLastName
        ...)             

    // adding secured info, in case of validated chats creation:
    info.securedInfo = "..."  

    // Create a chat that relates to an existing user:
    info.id = VISITOR_ID
    //OR
    info.visitorId = VISITOR_ID  
}
```

### Listening to account updates
In order to get account session updates, like `visitorId`, `chatId`, details that were filled by the user on the prechat, etc, an implementation of [`AccountInfoProvider`]({{'/docs/chat-configuration/setting-account/account-info-provider' | relative_url}}) should be set on `Chatcontroller.Builder`.
Once session data gets updated, the `AccountInfoProvider.update` method is called. 
```kotlin
val chatController = ChatController.Builder(context) 
                    .accountProvider(accountInfoProviderImpl)
                    ...
                    .build(account,...)

// on accountInfoProvider impl:
override fun update(account: AccountInfo) {
    // fetching chatId:
    account.info.chatId

    // fetching visitorId:
    account.info.visitorId 
}
```
---

## Start a live chat
{: .d-inline-block}
The SDK provides the option to start a live chat, or escalate to live chat from a chat with AI.

### Start Bold live chat
- Create a [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) with BoldAccount.

    ```kotlin
    val account = BoldAccount(API_KEY)
    val chatController = ChatController.Builder(context)                                                     
                                        .build(account, ...)
    ```
    

- Use an existing `ChatController` and call [`chatController.startChat`]({{'/docs/chat-configuration/extra/chatcontroller#start-chats' | relative_url}})/`chatController.restoreChat` with a BoldAccount.
    ```kotlin
    val account = BoldAccount(API_KEY)
    chatController.startChat(account)
    // or
    chatController.restoreChat(account = account)
    ```

![]({{'/assets/diagrams/start_with_live.png' | relative_url}})

### Escalate to Bold live chat from chat with AI
Chat escalation is done when the user selects a [Chat typed channel](https://support.nanorep.com/API-Integrations/Chat-Integration/1009694282/How-to-integrate-LiveChat-Inc-chat.htm) configured on the Bold360ai console, from a bot response options.

In case the hosting App implements the AccountInfoProvider, the [`AccountInfoProvider.provide`]({{'/docs/chat-configuration/extra/account-info-provider#account-provide' | relative_url}}) method will be called with a basic BoldAccount, according to the data provided from the channel. The `provide` method call, enables the App to [configure](#configure-chat-session) what ever needed for the live chat session.   
> If `AccountInfoProvider` implementation was not provided, the chat will start with the created BoldAccount as is.
    
![]({{'/assets/diagrams/bot_to_live.png' | relative_url}})

--- 

## Live Chat continuity
set the visitorId

---

## How to

- Define chat branding strings language.   
`account.info.language = "FR-fr"`

- Define the id of a specific department that should receive the chat.    
`account.info.department = DEPARTMENT_ID`

- Create a chat without displaying the prechat form.
`account.info.skipPrechat = true` or `account.skipPrechat()`   
  ##### _Notice: If the prechat form is being skipped, you can still send user details via extraData._
  {: .no_toc}
