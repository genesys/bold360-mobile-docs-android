# ChatController
The SDK's instrument to create and restore chats.
The `ChatControler` is created via a builder, which can be configured with providers and listeners as preferred. Those will be applied to the ChatController and to the chat participating components, on build time.

Once created, the `ChatController` can be used for multiple chats creation.

It is up to the `ChatController` creator, to manage its destruction, once done using.   
On `ChatController` destruction, all open chat sessions will be closed.   
Destructed `ChatController` can no longer be used to create chats, and will throw an exception if one try to.
```kotlin
chatController.destruct()
```

## Creating chats
- #### Using the ChatController.Builder
  Create a new `ChatController` for every chat creation, with the required account.
  ```kotlin
  val chatController = ChatController.Builder(context).apply {
                            conversationSettings(...)
                            chatEventListener(...)
                            ...
                        }.build(account, ChatLoadedListener{})
  ```

- #### Using same `ChatController` with account 
  ```kotlin
  chatController.startChat(account)
  // same as:
  chatController.restoreChat(account = account, autoChatEnd = false)
  ```
  If you want to provide your created `NRConversationFragment` or configure `autoChatEnd` flag use:

  ```kotlin
  // start a new chat session with account and fragment configured as manual destruction.
  chatController.restoreChat(fragment = chatFragment, account = account, autoChatEnd = false)

  // restore last chat session configured as manual destruction.
  chatController.restoreChat(autoChatEnd = false)
  ```
  > `autoChatEnd` flag (defaults to true) - defines if the current chat session should be ended automatically, when the fragment is destroyed.   

  If chat was configured as `manual destruction`, it is up to the chat creator, to end the chat session.
  ```kotlin
  chatController.endChat()
  ```

When chat starts, if `ChatElementListener` implementation is provided, the previous chat history will be displayed on the chat window.
