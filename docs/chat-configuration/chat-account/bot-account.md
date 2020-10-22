---
layout: default
title: BotAccount
parent: Chat Account
grand_parent: Chat Configuration
nav_order: 1
permalink: /docs/chat-configuration/chat-account/bot-account
---

# BotAccount
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

Use this account to create chat sessions with AI.

### Creating account

```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER)
```  

- API_KEY - As was configured to your account.
- ACCOUNT_NAME <sub>[mandatory]</sub> - As was configured to your account.
- KNOWLEDGE_BASE <sub>[mandatory]</sub> - The knowledge base you would like this chat to work with.
- SERVER <sub>[mandatory]</sub> - As was configured to your account.

### Configure Contexts

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

### Configure [Initialization Entities](/docs/advanced-topics/entities-and-personal-info#initentities)
 ```kotlin
 account.initializationEntities = mapOf("EntityKey1" to "EntityValue1",
                                     "EntityKey2" to "EntityValue2", ... )
 ```
 ---

### How to
- ##### Customize and override chat [Welcome Message](/docs/chat-configuration/extra/welcome-message)

    ```kotlin
    botAccount.welcomeMessage = ARTICLE_ID 
    // if id is not valid, no message will be displayed
    
    botAccount.welcomeMessage = BotAccount.None 
    // prevent welcome message display                 
    ```

