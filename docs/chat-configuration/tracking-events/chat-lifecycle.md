---
layout: default
title: Chat Lifecycle
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 1
# permalink: /docs/chat-configuration/tracking-events/chat-lifecycle
---

# Chat Lifecycle
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
Through chat progress, it can be in various states, each has it's own meaning. The hosting app can react to chat states and control it's flow, by listening to chat states changes.   
{: .overview}
In order to listen to chat lifecycle events, set your implemention of `ChatEventListener`, to the `ChatController`.

```kotlin
// on ChatController construction:
val chatController = ChatController.Builder(context)
                        .chatEventListener(listener)...build(...)

// or later in time:
chatController.chatEventListener = listener
```
---

## Lifecycle Events

| Name      | Description  |
|:---|:---|
| Preparing<br>`StateEvent.Preparing` | Passed when chat creation is starting.  |
| Created<br>`StateEvent.Created`   | Passed when the chat was successfully created |
| Started<br>`StateEvent.Started`   | Passed when the chat was successfully started.<br><sup>**On live chat: the chat was not yet accepted by an agent.**</sup><br>At this point the User can start posting messages on the chat, those will be received by the accepting agent. |
| <a id="pending"/>Pending<br>`StateEvent.Pending`<br><sub>_Live chat only_</sub> | The chat was assigned to an agent, waiting for his acceptance.<br><sup>**Chat unavailable will be triggered, if timeout was configured and the chat was not accepted on time.**</sup> |
| InQueue<br>`StateEvent.InQueue`<br><sub>_Live chat only_</sub> | The chat is in a waiting queue, waiting to be assigned to an agent. The chat position in the queue is passed was well.  |
| <a id="accepted"/>Accepted<br>`StateEvent.Accepted`<br><sub>_Live chat only_</sub> | Passed when an agent accepts the chat.   |
| Reconnected<br>`StateEvent.Reconnected`<br><sub>_Messaging chat only_</sub> | Passed when the chat got reconnected after connection lost. |
| Unavailable<br>`StateEvent.Unavailable`<br><sub>_Live chat only_</sub> | Passed when chat initiation was rejected due to unavailability. (no operators, out of working hours, etc.)
| Ending<br>`StateEvent.Ending`<br><sub>_Live chat only_</sub> | Passed when the chat started ending process.<br>Postchat form data will be available on this event.       |
| Ended<br>`StateEvent.Ended` | Passed when the chat ends. |
| Idle<br>`StateEvent.Idle` | Passed when there are no more active chats in the ChatController. |
| ChatWindowLoaded<br>`StateEvent.ChatWindowLoaded` | will be triggered once the chat fragment was loaded and ready to display chat elements and receive injections. |
| ChatWindowDetached<br>`StateEvent.ChatWindowDetached` | Will be triggered once the chat window was closed and should be removed from its containing activity. |

---

<details open markdown="block">
<summary>States handling code sample:</summary>

```java
    @Override
    public void onChatStateChanged(@NotNull StateEvent stateEvent) {
        switch (stateEvent.getState()) {
            case StateEvent.Started:
                // do something
                
            case StateEvent.Unavailable:
                // do something
                
            case StateEvent.Ended:
                // do something
                
            case StateEvent.InQueue:
                // fetching the queue position from the event:
                InQueueEvent queueEvent = (InQueueEvent) stateEvent;
                int queuePosition = queueEvent.getPosition();
                
            case StateEvent.Idle:
                // no more active chats, we can remove chat fragment
                
            case StateEvent.ChatWindowLoaded:
                // do something
                
            case StateEvent.ChatWindowDetached:
                // removing chat fragment from its activity
        }
    }
```

</details>