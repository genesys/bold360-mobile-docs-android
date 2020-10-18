---
layout: default
title: ChatController
nav_exclude: true
---

# ChatController
{: .no_toc }



---

## Overview



**To end current chat** use `chatcontroller.endChat` method. If you don't want to display the postChat form activate this method with forceClose = true, `chatController.endChat(true)`   

**To end All chats at once** use `chatController.terminateChat` method. this will end all active chats.
an event will be passed to `onChatStateChanged` `StateEvent.ChatWindowDetached` indicating that chat fragment should be removed since we're no longer has an active chat.   
   