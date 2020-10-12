---
layout: default
title: Chat Continuity
parent: Advanced Topics
nav_order: 2
permalink: /docs/advanced-topics/chat-continuity
has_toc: false
---

# Chat Continuity
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. [Chat Session](/docs/advanced-topics/chat-continuity#chat_session)
1. [AccountInfoProvider](/docs/chat-configuration/setting-account/account-info-provider)
2. [Messaging chat continuation](./AsyncChatContinuation.md)
3. [Bold live chat continuation](./BoldChatContinuation.md)
4. [Bot ai chat continuation](./BotChatContinuation.md)

---

The option to relate and "continue" chat sessions. 

## Chat Session
Each chat type (bot, live, Messaging) has its own properties that are being used to enable continuity.   
Those properties are configurable on the accounts `info: SessionInfo` property.  


#### Listening to session data updates
While the chat is in progress, session data, like ids, may be created or changed.  
In order to be updated with such changes for later use, implement [_AccountInfoProvider_ or _AccountSessionListener_](./android-AccountInfoProvider.md) and pass implementation on `ChatController` creation. 
```kotlin
val chatController = ChatController.Builder(context).apply{
                        accountProvider(MyProvider)
                     }               
                    .build(account, ...)
```

