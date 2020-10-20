---
layout: default
title: ChatController
nav_exclude: true
---

# ChatController
{: .no_toc }

need work
{: .btn .btn-orange }

---

## Overview
{: .overview}


## ChatController
The SDK's API to manage chats. Create a `ChatController` instance to create, restore and interact with chats. Once chat needs to be closed, use the ChatController to do that.   
- The `ChatControler` is created via a builder, which can be configured with providers and listeners which defines the created chats behavior and interaction with the embedding App.  
- In order to close open chats use chatController.terminateChat() or chatController.endChat() 
- In order to release the ChatController's resources use `ChatController.destruct()`   
  - **Destruction of unneeded ChatController instance, is very important, in order to close and release open communication components.**
  - ChatController destruction, releases resources and listeners, but does not ends the open chats. Ending of chat should be done by the embedding App.   
  - Destructed `ChatController` can no longer be used to create chats, and throws an exception if one tries to.


**To end current chat** use `chatcontroller.endChat` method. If you don't want to display the postChat form activate this method with forceClose = true, `chatController.endChat(true)`   

**To end All chats at once** use `chatController.terminateChat` method. this will end all active chats.
an event will be passed to `onChatStateChanged` `StateEvent.ChatWindowDetached` indicating that chat fragment should be removed since we're no longer has an active chat.   
   