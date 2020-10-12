---
layout: default
title: BotAccount
parent: Setting Account
grand_parent: Chat Configuration
nav_order: 1
permalink: /docs/chat-configuration/setting-account/bot-account
---

# BotAccount
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


Use this account to create chat sessions with AI.

```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER)
```  

- API_KEY - As was configured to your account.
- ACCOUNT_NAME <sub>[mandatory]</sub> - As was configured to your account.
- KNOWLEDGE_BASE <sub>[mandatory]</sub> - The knowledge base you would like this chat to work with.
- SERVER <sub>[mandatory]</sub> - As was configured to your account.

- If your account is configured to use contexts, provide the list of contexts as follows:

    ```kotlin
    val contexts = mapOf("ContextKey1" to "ContextValue1",
                        "ContextKey2" to "ContextValue2",
                        ... )

    val account = BotAccount(API_KEY, ACCOUNT_NAME,
                            KNOWLEDGE_BASE, SERVER, contexts)


    // OR 
    val account = BotAccount(API_KEY, ACCOUNT_NAME,
                            KNOWLEDGE_BASE, SERVER)
    ...                            
    account.contexts = contexts
    ```

- If the welcome message should be customized and override current console configurations, create account as follows:

    ```kotlin
    val account = BotAccount(API_KEY, ACCOUNT_NAME, KNOWLEDGE_BASE,
                            SERVER, CONTEXT_MAP).apply {
                                welcomeMessage = ARTICLE_ID
                            }
    ```

- If the account is using Initialization Entities, please follow this [link](./Personal_Information.md#Initialization_entites)
