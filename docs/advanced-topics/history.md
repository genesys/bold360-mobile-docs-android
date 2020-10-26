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

The SDK enables the Host app to provide its own implementation, and maintain an updated chat history, by listening to triggered events whenever chat elements changes.
{: .overview}

## How is it done
The hosting App provides a `ChatElementListener` implementation, on ChatController creation. This listener should be fully implemented in order to be notified of all chat changes.   
The SDK interacts with this listener in order to fetch chat history on chat load, and to update history records, when chat elements were changed.

### ChatElementListener overview
The `ChatElementListener` will be used for the following operations.
- Fetch chat elements from stored history on chat load.   
Calling `ChatElementListener.onFetch`

- Notifies of an addition of chat element to the chat.  
Calling `ChatElementListener.onReceive`

- Notifies of removal of stored element from the chat.   
Calling `ChatElementListener.onRemove`

- Notifies of an update on chat element data.  
Calling `ChatElementListener.onUpdate`

### ![]({{'assets/images/iconfinder_ic_info.png' | relative_url}}) Guidelines
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

























