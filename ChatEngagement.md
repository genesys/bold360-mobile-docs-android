# Chat Engagement
The SDK exposes API to inject messages into the chat.  
Messages injection is allowed once chat was **Accepted**.   
Implement `ChatEventListener::onChatStateChanged` in order to [get chat states](./ChatLifecycleEventsAndroid).

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
