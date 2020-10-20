---
layout: default
title: Chat Restoring
parent: Advanced Topics
nav_order: 3
permalink: /docs/advanced-topics/chat-restore
---

# Chat Restoring
{: .no_toc }
need work
{: .btn .btn-orange }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
A key player in chat restoring is the `ChatController`.
{: .overview}

## ChatController
The SDK's API to manage chats. Create a `ChatController` instance to create, restore and interact with chats. Once chat needs to be closed, use the ChatController to do that.   
- The `ChatControler` is created via a builder, which can be configured with providers and listeners which defines the created chats behavior and interaction with the embedding App.  
- In order to close open chats use chatController.terminateChat() or chatController.endChat() 
- In order to release the ChatController's resources use `ChatController.destruct()`   
  - **Destruction of unneeded ChatController instance, is very important, in order to close and release open communication components.**
  - ChatController destruction, releases resources and listeners, but does not ends the open chats. Ending of chat should be done by the embedding App.   
  - Destructed `ChatController` can no longer be used to create chats, and throws an exception if one tries to.


## How to create and restore chats
- ### Create chat using the ChatController.Builder
  This will create a new ChatController instance and will automatically starts a chat according to the provided account.
  ```kotlin
  val chatController = ChatController.Builder(context).apply {
                            conversationSettings(...)
                            chatEventListener(...)
                            ...
                        }.build(account, ChatLoadedListener{})
  ```

- ### Create and restore chats using existing `ChatController` instance
  - **Start a new chat -**   
`chatController.startChat(account)`  
  All current open chats will be released.   
  Released but **not closed**, the embedding App should close open chats, when done.

  - **Restore a chat** -    
`chatController.restoreChat(Fragment?, AccountInfo?)`   
    <u>Usages:</u>
    - On Activity restoring, restore the chat with a restored chat fragment:
      ```kotlin
      // Restore and continue current active chat (if available) with provided fragment:
      chatController.restoreChat(fragment = chatFragment)      

      // start a new chat session with provided account and fragment:
      chatController.restoreChat(fragment = chatFragment, account = account)
      ```
  
    - Continue with an active chat after its UI was removed:
      ```kotlin
      // continue current active chat:
      chatController.restoreChat()
      ```
    
    - Start a new chat-
      ```kotlin
      // If account is provided, will be considered as starting a new chat:
      chatController.restoreChat(account)
      ```

