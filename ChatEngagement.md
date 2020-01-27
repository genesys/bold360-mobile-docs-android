# Chat Engagement
The SDK exposes API to inject messages into the chat.

## How to inject messages:

Use `ChatController.post(...)` as follows:

```kotlin
// inject message from user side:
val outgoing = OutgoingStatement("hello")
chatController.post(outgoing);

// inject message from agent side:
val incoming = IncomingStatement("Hi")
chatController.post(incoming);

// inject system message:
val system = SystemStatement("This is an automatic message")
chatController.post(system);
```
