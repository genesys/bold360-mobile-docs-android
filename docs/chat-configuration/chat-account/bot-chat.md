---
layout: default
title: AI Chat
parent: Chat Account
grand_parent: Chat Configuration
nav_order: 1
---

# Chat with AI <sup>chatbot</sup> 
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The SDKs chat with AI bridges your APP with the conversational platform of Bold360 ai. Provides a messaging-based user experience in which the Bold360 ai engine gathers information from the customer in a way similar to what a human agent would do in a regular conversation. Based on customer input and the engine's search capabilities, the Conversational Bot provides the customer relevant information.
{: .overview}

The main key for creating a chat is the account.

## BotAccount
Use this account to create chat sessions with AI.

With the account you can configure the chatbot that will be created, provide extra details when needed and define other configurations for the chat session.

### Creating account

```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER)
```  

- API_KEY - As was configured to your account.
- ACCOUNT_NAME <sub>[mandatory]</sub> - As was configured to your account.
- KNOWLEDGE_BASE <sub>[mandatory]</sub> - The knowledge base that should be used for this chat.
- SERVER <sub>[mandatory]</sub> - As was configured to your account.

### Configure UserId
The UserId is used for reports and analitics. UserId should be managed and applied by the application on the chat creation, if not provided, the SDK creates a new id. In order to relate analitic content to the same user, the same UserId should be provided.

```kotlin 
val account = BotAccount(...).apply{
    userId = SAVED_USER_ID
}
```

> Hosting application can get the newly created UserId on [`AccountInfoProvider.update`]({{'/docs/chat-configuration/extra/account-info-provider#account-update' | relative_url}}) and keep it for later use.
```kotlin 
override fun update(account: AccountInfo) {
    account.userId
}
```


### Configure Contexts
If your account supports context based answers, you may want to configure those on your account to be used during the chat.

```kotlin
// Create contexts map 
val contexts = mapOf("ContextKey1" to "ContextValue1",
                    "ContextKey2" to "ContextValue2", ... )

// set on constructor:
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER, contexts)

// or later in time, before chat creation:                           
account.contexts = contexts
```

### Configure [Initialization Entities]({{'/docs/advanced-topics/entities-and-personal-info#initentities' | relative_url}})
Custom Entities, are pices of information that should be provided by the application/user during the chat, instead of using a form, we use the conversation to gather those details.
Some entities are static and are not going to change during the chat, therefore can be provided on the chat creation. Those entities are provided on the account.

 ```kotlin
 account.initializationEntities = mapOf("EntityKey1" to "EntityValue1",
                                     "EntityKey2" to "EntityValue2", ... )
 ```
 ---

## AI Chat continuity
Chatbot continuity has no special meaning as in live chat. 
Previous conversationId can be provided, for the initiated chat session, and if valid will be used, otherwise a new chat session will be created.
The created  conversationId is available for Hosting app managment via [`AccountInfoProvider.update`]({{'/docs/chat-configuration/extra/account-info-provider#account-update' | relative_url}}).
```kotlin 
override fun update(account: AccountInfo) {
    account.info.id // conversationId
}
```

---


## How to
- ### Customize and override chat [Welcome Message]({{'/docs/chat-configuration/extra/welcome-message' | relative_url}})
  Bold360 console configured welcome message article, can be overridden, or provided if none configured, on the account. 

    ```kotlin
    botAccount.welcomeMessage = ARTICLE_ID 
    // if id is not valid, no message will be displayed

    botAccount.welcomeMessage = BotAccount.None 
    // prevent welcome message display                 
    ```

