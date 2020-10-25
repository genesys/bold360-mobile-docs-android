---
layout: default
title: Messages Injection
parent: Advanced Topics
nav_order: 4
# permalink: /docs/advanced-topics/messages-injection
---

# Messages Injection {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Welcome message](/docs/chat-configuration/extra/welcome-message)
- [FAQs message](/docs/chat-configuration/extra/faqs-message)

---


## Overview
The hosting app has the ability to control and engage with the messages that are being inserted to the chat.  
Messages of vary types can be injected into the chat, while some may be configured to be injected by the SDK.  
{: .overview}

## Injecting chat messages
The SDK exposes an API to enable injection of messages into the chat by the hosting app.  
Messages can be injected to the chat, once the chat was [**Accepted**](/docs/chat-configuration/tracking-events/chat-lifecycle#accepted). 

```kotlin
override fun onChatStateChanged(stateEvent: StateEvent) {
    when (stateEvent.state) {
        StateEvent.Accepted -> {
            // now you can start injecting messages to the chat 
            chatController.post(...)
        }
        ...
    }
}
```

## How to inject messages:

Use `ChatController::post` method.

Examples:
```kotlin
// inject message from user side:
chatController.post(OutgoingStatement("hello"));

// inject message from agent side:
chatController.post(IncomingStatement("Hi"));

// inject system message:
chatController.post(SystemStatement("This is an automatic message"));
```