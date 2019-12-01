
# Bold chat Queue

This article will explain how to enable queue component and configure it.

When user starts a bold chat session, until an agent is assigned to the user's chat and accept it, the chat can be added to a queue.
 
When previous chats will end the user's chat will be promoted in the queue until position 0, means the chat is no longer in the queue and the chat is waiting for agent acceptance.

> User can end the chat while in the queue with [life cycle `ChatState`](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_ios_adv_chat_lifecycle.html) (`ChatInQueue`) or while waiting for acceptance (`ChatPending`). 
The user will be presented with unavailability form if enabled otherwise a message.

## How to enable queue component

1. Enable `[automatic distribution]` in bold console.   
Some configurations can be changed, like queue limit, agent concurrent chat limit etc.
2. Enable `[Show queue position]` in bold console, in order to be notified of user position in queue.

## Queue Component Display

Queue position UI display is provided by the SDK. 
>The component can be configured (text size font etc).

## How to Configure the Queue Component

To configure queue component use `QueueViewConfiguration` in following way:

```swift
self.chatController.viewConfiguration.queueViewConfig.backgroundColor = UIColor.red
```

