---
layout: default
title: User Tracking
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 5
permalink: /docs/chat-configuration/tracking-events/user-tracking
---

# User tracking {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Tracking user activity
In order to track user activity during chat session, a userId should be provided on the account object.   
On the first account chat session, the userId should remain null. The SDK generates by a BE API, a unique token, that should be saved by the embedding app, and should be provided from this point forward for all chats of this account.   
This token enables the tracking of user activity.

## How to get the generated userId
Once the chat is created, an account update event is triggered on the `AccountInfoProvider`, the userId can be taken from the provided updated account.
> Read more on [AccountInfoProvider](./android-AccountInfoProvider.md)

```kotlin
class SimpleAccountProvider : AccountInfoProvider {

    var accounts: MutableMap<String, AccountInfo> = mutableMapOf()

    override fun update(account: AccountInfo) {
        accounts[account.getApiKey()]?.userId = account.userId
        // saved userId should be used on future chats creation for this account
    }
}
```

## Create a chat with the generated userId

The `userId` should be provided on the `BotAccount`.
```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                KNOWLEDGE_BASE, SERVER, CONTEXT_MAP).apply {
                        
                        userId = SAVED_GENERATED_ID
                } 
                
ChatController.Builder(context)...build(account, ...)
```

