# Live chat components

## Chatbar component
Once a **Bold live chat** is started a `Chatbar` component will be displayed over the chat.   
The Chatbar contains:
 1. Current live agent's name an avatar.
 2. `End Chat` clickable view to end current live chat.

##
### How to customize

- Component can be configured or/and overrided via `ChatBarCmpUiProvider`. 
```kotlin
ChatUIProvider(this).apply {
    chatBarCmpUiProvider.configure = { adapter ->
        adapter.configAgentCmp(ChatbarCmpConfig().apply { 
            this.textStyle = StyleConfig(...)
        })
        adapter.configEndCmp(ChatbarCmpConfig().apply {...})
        adapter.setBackground(...)
        adapter
    }
}
```
Find more in [ Customizing UI components](https://github.com/bold360ai/GlobalDocs/wiki/AndroidChatCustomizations).

- Agents avatar placeholder image can be override, by overriding the relevant drawable resource. (agent.png)
- In order to disable the "end chat" display, use the `ConversationSettings`.
    ```kotlin
    ConversationSettings()...
            .disableEndLiveChatSupport()
    ```

___
## Queue component

When user starts a bold chat session, until an agent is assigned to the user's chat and accept it, the chat can be added to a queue.
 
When previous chats will end the user's chat will be promoted in the queue until position 0, means the chat is no longer in the queue and the chat is waiting for agent acceptance.

> User can end the chat while in the queue with [life cycle state](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_android_adv_chat_lifecycle.html) (`StateEvent.InQueue`) or while waiting for acceptance (`StateEvent.Pending`). The user will be presented with unavailability form if enabled otherwise a message.

### How to enable support
1. Enable `[automatic distribution]` in bold console.   
Some configurations can be changed, like queue limit, agent concurrent chat limit etc.
2. Enable `[Show queue position]` in bold console, in order to be notified of user position in queue.

### How to customize
Queue position UI display is provided by the SDK, and can be customize either by configuration setting or by view implementation overriding.

- #### Customizing by Configure
    Override `ChatUIProvider.queueCmpUIProvider.configure` method:
    ```kotlin
    ChatUIProvider(context).apply {
        queueCmpUIProvider.configure = { adapter:QueueCmpAdapter -> 
            ...
        }
    }
    ```
- #### Customizing by override
   Override overrideFactory value with your own `QueueFactory` implementation:
    ```kotlin
    ChatUIProvider(context).apply {
        queueCmpUIProvider.overrideFactory = MyQueueFactory()
    }

    // for exp:
    class MyQueueFactory : QueueFactory{
        // QueueCmpAdapter interface should be 
        // implemented by the overriding component
        return object : QueueCmpAdapter{
            ...
        }
    }

    ```

___

## Typing indication component
Appears when the live agent is typing.  
The typing indication can be displayed as text and/or drawable.   
Typing drawable can be an [AnimatedVector](https://developer.android.com/reference/android/graphics/drawable/AnimatedVectorDrawable).

### How to enable
By default typing indication is `enabled`.   
The component display can be disabled through `ConversationSettings`.
```kotlin 
val settings =  ConversationSettings()...
        .disableTypingIndication()

ChatController.Builder(context).apply {
    conversationSettings(settings)
    ...
}
```

### Listening to typing notifications

In order to receive typing state changes, subscribe to `OperatorEvent.OperatorTyping' notifications, with the chatController.
```kotlin
// 1. implement Notifiable interface
// 2. pass the implementation and the list of notifications that you want to receive.  
chatController.subscribeNotifications(NotifiableImpl, OperatorEvent.OperatorTyping,...)
```

<sup> <b> [more on notifications ](https://github.com/bold360ai/GlobalDocs/wiki/Listeners-and-subscriptions-android) </b></sup>

### How to customize

Typing indication UI component is provided by the SDK, and can be customize either by configuration setting or by view implementation overriding.
- #### Customizing by Configure
    Override `ChatUIProvider.typingUIProvider.configure` method:
    ```kotlin
    ChatUIProvider(context).apply {
        typingUIProvider.configure = { adapter:TypingUIAdapter -> 
            ...
        }
    }
    ```
- #### Customizing by override
   Override overrideFactory value with your own `TypingFactory` implementation:
    ```kotlin
    ChatUIProvider(context).apply {
        typingUIProvider.overrideFactory = MyTypingFactory()
    }

    // for exp:
    class MyTypingFactory : TypingFactory{
        // TypingUIAdapter interface should be 
        // implemented by the overriding component
        return object : TypingUIAdapter{
            ...
        }
    }

    ```
