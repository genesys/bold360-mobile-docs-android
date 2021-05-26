---
layout: default
title: ChatController
nav_exclude: true
---

# ChatController
{: .no_toc }

---

## Overview
Bridging interface between the hosting App and the chat SDK. The ChatController provides APIs to enable hosting Apps to create and manage chats.
{: .overview}

## How to use
The `ChatControler` is created with a builder class. The builder can be configured with providers and listeners, that will be used by the ChatController during the chat progress, to pass events and notifications to the hosting App and to trigger App's actions in some cases.   
ChatController creation also triggers the creation and start of the configured chat as was provided to the Builder.build, AccountInfo parameter. 
The ChatController instance may be used for multiple chats creation.  

> 👉 First chat starts with the ChatController creation, <u>no need to call </u>`startChat`<u> to activate it.</u>   


### Start chats
After ChatController instance creation, chats can be started with `startChat`, which defines a new chat, or with `restoreChat` that can be used to continue a previous ongoing chat.

### End chats
With the ChatController instance, the hosting App can end active chats.

- Use `endChat`, to end the current active chat.   
  
  `forceClose = true` can be passed, for fast chat closure. When force closing live chat, postchat form will not be activated.
  {: .mt-2 .eg-class}

- Use `terminateChat`, to end **all** current active chats. 
  
  e.g., Bot chat was escalated to live chat. In order to close both bot and escalated chats we use `chatController.terminateChat()`
  {: .mt-2 .eg-class}

- Once the ChatController has no more active chats, it passes a `StateEvent.Idle`, to its attached `ChatEventListener` implementation.

### Destruct and release
In order to release the ChatController's resources, use `ChatController.destruct()`.  
Destructed `ChatController` instance **can no longer be used to create/manage chats**. Any usage attempt 
will end with `IllegalStateException`.

> ChatController destruction, releases resources and listeners, but does not ends the open chats. Ending active chats (`terminateChat`) should be activated by the hosting App prior to ChatController destruction.   
