---
layout: default
title: BotAccount
parent: Setting Account
grand_parent: Chat Configuration
nav_order: 1
permalink: /docs/chat-configuration/setting-account/bot-account
---

# BotAccount

```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER, CONTEXT_MAP)
```  

Where: API_KEY (mandatory), ACCOUNT_NAME(mandatory), KNOWLEDGE_BASE(mandatory), SERVER(mandatory), CONTEXT_MAP(optional)

- If the account is using Context, create BotAccount as follows:

    ```kotlin
    val contexts = mapOf("ContextKey1" to "ContextValue1",
                        "ContextKey2" to "ContextValue2",
                        ... )

    val account = BotAccount(API_KEY, ACCOUNT_NAME,
                            KNOWLEDGE_BASE, SERVER, contexts)
    ```

- If the welcome message should be customised and override current console configurations, create account as follows:

    ```kotlin
    val account = BotAccount(API_KEY, ACCOUNT_NAME, KNOWLEDGE_BASE,
                            SERVER, CONTEXT_MAP).apply {
                                welcomeMessage = ARTICLE_ID
                            }
    ```

- If the account is using Initialization Entities, please follow this [link](./Personal_Information.md#Initialization_entites)
