---
layout: default
title: ChatController
nav_exclude: true
---

# ChatController  {{site.data.vars.need-work}}
{: .no_toc }

---

## Overview
Bridging interface between the hosting App and the chat SDK. The ChatController defines the SDK APIs to enable hosting Apps to create and manage chats.
{: .overview}

## How to use
The `ChatControler` is created with a builder class. The builder can be configured with providers and listeners, attached to the chats and and enables the hosting App to interact and react while the chat is in progress.  
The ChatController instance may be used for multiple chats creation.   

### Start chats
Chats can be started with `startChat`, which defines a new chat, or with `restoreChat` that can be used to continue a previous ongoing chat.

### End chats
With the ChatController instance, the hosting App can end active chats.

- Use `endChat`, to end the current active chat.   
  > Ending Live chat: If you don't want to display the postChat form activate this method with forceClose = true

- Use `terminateChat`, to end **all** current active chats. 
  > e.g., Bot chat escalation to live chat, in order to close both active chats, the escalated and the bot source chat, use `chatController.terminateChat()`  
  Once the ChatController has no more active chats, it passes a `StateEvent.Idle`, to its attached `ChatEventListener` implementation.

### Destruct and release
In order to release the ChatController's resources, use, and the instance itself, use `ChatController.destruct()`   
Destructed `ChatController` instance can no longer be used to create/manage chats. Any usage attempt 
will end with `IllegalStateException`.

> ChatController destruction, releases resources and listeners, but does not ends the open chats. Ending of chat should be done by the embedding App prior destruction.   
