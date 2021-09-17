---
layout: default
title: Chat Continuity
parent: Advanced Topics
nav_order: 2
has_toc: false
---

# Chat Continuity {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [AccountInfoProvider]({{'/docs/chat-configuration/extra/account-info-provider' | relative_url}})
- [Messaging chat continuity]({{'/docs/advanced-topics/messaging-chat/#messaging-chat-continuity' | relative_url}})
- [Live chat continuity]({{'/docs/chat-configuration/chat-account/bold-chat/#live-chat-continuity' | relative_url}})
- [AI chat continuity]({{'/docs/chat-configuration/chat-account/bot-chat/#ai-chat-continuity' | relative_url}})

---

## Overview
The ability to relate chats to user previous chats or "continue" chat sessions.   
Each chat, bot, live, Messaging, has its meaning for continuity. Messaging chat can actually be continued while live chat can be related to previous user chats history.
{: .overview}

## Chat Session
The chat session is being created by the ChatController. On ChatController creation or when `chatController.startChat()` or `chatController.restoreChat()` were activated with an account object.   

Each chat type, has account session properties that should be configured to enable continuity. Those properties can be configured on the account `info: SessionInfo` property.   
<sup>Explore [specific chat contiuity](#table-of-contents) </sup> 

#### Listening to session data updates
While the chat is in progress, session data, like ids, may be created or changed.  
In order to be updated with such changes for later use, implement [_AccountInfoProvider_]({{'/docs/chat-configuration/extra/account-info-provider' | relative_url}}) or [_AccountSessionListener_]({{'/docs/chat-configuration/extra/account-info-provider#ongoing-session-configurations-updates' | relative_url}}) and pass implementation on `ChatController` creation. 
```kotlin
val chatController = ChatController.Builder(context).apply{
                        accountProvider(MyProvider)
                     }               
                    .build(account, ...)
```

