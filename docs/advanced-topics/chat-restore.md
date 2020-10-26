---
layout: default
title: Chat Restoring
parent: Advanced Topics
nav_order: 3
---

# Chat Restoring {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
A key player in chat restoring is the [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}).
{: .overview}

---

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
  - Start a new chat 
  {: .strong-sub-title}  
   `chatController.startChat(account)`  
    All current open chats will be released.   
    Released but **not closed**, the embedding App should close open chats, when done.

  - Restore a chat
  {: .strong-sub-title}

  `chatController.restoreChat(Fragment?, AccountInfo?)`   
  
    Usages:
    {: .light-title }
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

