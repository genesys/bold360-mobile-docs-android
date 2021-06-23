---
layout: default
title: Chat History
parent: Advanced Topics
nav_order: 1
# permalink: /docs/advanced-topics/history
---

# Chat History {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
> ##### History management is out of the SDKs scope.  

The SDK enables the hosting app to provide its own implementation, and maintain an updated chat history, by listening to elements related triggered events.   
The SDK interacts with the hosting app's provided implementation, during chat progression. Fetches stored elements and updates with certain methods to keep an accurate view of the chat elements state.
{: .overview}

### Listening to events with ChatElementListener
The hosting App sets a [`ChatElementListener`]({{'/docs/chat-configuration/tracking-events/events-and-notifications/#chat-elements-events' | relative_url}}) implementation, to the `ChatController`.   
In order to be updated with all changes, and maintain an updated chat history, the provided `ChatElementListener` implementation, should implement, at least, the following methods:

- `onFetch`: Fetches chat elements from history implementation storage when the chat loads.   

- `onReceive`: Notifies of a `StorableChatElement` addition to the chat. 

- `onRemove`: Notifies of a chat element removal from the chat. 

- `onUpdate`: Notifies of an update to chat element data. 

**Only changes on `StorableChatElement` implementing elements, will trigger the above mentioned methods.**
### 📚  Guidelines
- The provided history management implementation decides, if chat history fetching is done by paging, and if so, what the page size should be, or fetch of one block with all messages.  
On chat load,  the SDK requests the first history block, and display it on the chat, if was provided. As long as the user keeps scrolling upward to see older messages, the SDK will continue its requests for more history blocks, until an empty block is received, which indicates, that no more history is available for this chat.

- Once a messages block was fetched from the history implementation, it should be passed to the SDK via the `HistoryCallback` instance provided on the fetch request.

- The SDK does not forces the hosting App to implement and react to chat changes in a specific way.  
The chat changes notifications can be used for history saving or just for logs.  
The history implementation defines which messages should be kept for history purposes.

- Chat elements that can be stored are identified as `StorableChatElement`.   
A `StorableChatElement` instance has an important property `storageKey` that should not be changed nor deleted, otherwise fetched messages will appear as broken.   

- Currently `StorableChatElement` items are identified by an `id` property.


### Attaching the history implementation 
```kotlin
class ChatElementListeningImpl : ChatElementListener{
    override fun onReceive(item: StorableChatElement) {
        // add item to chat history
    }

    override fun onRemove(timestampId: Long) {
        // remove item from chat history
    }

    override fun onUpdate(timestampId: Long, item: StorableChatElement) {
        // update item in chat history
    }

    override fun onFetch(from: Int, direction: Int, callback: HistoryCallback?) {
        // fetch page/all of history items starting from "from"
    }
}

// set implemenation on ChatController creation
ChatController.Builder(context)....
    .chatElementListener(chatElementListeningImpl)

// set on a built ChatController instance
chatController.setChatElementListener(chatElementListeningImpl)
```

























