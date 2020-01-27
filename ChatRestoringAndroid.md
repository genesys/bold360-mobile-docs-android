# ChatController
The SDK's tool to create and restore chats.
The `ChatControler` is created via a builder, which can be configured with providers and listeners as preferred. Those will be applied to the `ChatController` and to the chat participating components, on build time.

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
  chatController.restoreChat(account = account, endChatWithUI = false)
  ```
  If you want to provide your created chat fragment or configure `endChatWithUI` flag use:

  ```kotlin
  // start a new chat session with provided: account and fragment configured to be kept alive when UI was removed (manual destruction):
  chatController.restoreChat(fragment = chatFragment, account = account, endChatWithUI = false)

  // restore last created chat session, configured to be kept alive when UI was removed (manual destruction).
  chatController.restoreChat(endChatWithUI = false)
  ```
  > `endChatWithUI` flag (defaults to true) - defines if the current chat session should be ended automatically, when the fragment is destroyed.   
  If was set to false, it is up to the chat creator, to end the chat session.
  ```kotlin
  chatController.endChat()
  ```
