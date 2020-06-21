# Create and restore chats 
A key player in chat restoring is the `ChatController`.

## ChatController
The SDK's tool to create and restore chats.   
The `ChatControler` is created via a builder, which can be configured with providers and listeners as needed. Those, in turn, will be applied to the `ChatController` instance and other participating components.

Once created, the `ChatController` can be used for multiple chats creation.
Via the `ChatController` you can interact with the chat, and listen to its events.

`ChatController` destruction should be managed by its creator.   
```kotlin
chatController.destruct()
```   
- Once destruction was activated, all open chat sessions will be closed.   
- Destructed `ChatController` can no longer be used to create chats, and throws an exception if one tries so.


## How to create and restore chats
- ### Create chat using the ChatController.Builder
  Creates a new `ChatController` and a new chat, with the provided account.
  ```kotlin
  val chatController = ChatController.Builder(context).apply {
                            conversationSettings(...)
                            chatEventListener(...)
                            ...
                        }.build(account, ChatLoadedListener{})
  ```

- ### Create and restore chats using `ChatController` instance with a provided account 
  - Starts a new chat with the provided account. Closes all active chats if available.
    ```kotlin
    - kotlin:
    // Create a new chat, configured not to automatically be closed when chat UI gets destroyed.

    chatController.startChat(account)
    // same as:
    chatController.restoreChat(account = account)
    ```
  - In case of activity restoring, if you want to use the restored chat fragment use:

    ```kotlin
    // start a new chat session with provided account and fragment:
    chatController.restoreChat(fragment = chatFragment, account = account)

    // Restore and continue current active chat (if available) with provided fragment:
    chatController.restoreChat(fragment = chatFragment)
    ```
  
  - Configures chat to last passed UI destruction:
    ```kotlin
    // Create new chat:
    chatController.restoreChat(account, endChatWithUI = false)

    // Restore and continue current active chat:
    chatController.restoreChat(endChatWithUI = false)
    ```
    > <u>`endChatWithUI`</u> flag _(defaults to true)_ - defines if the current chat session should be ended automatically, when the chat fragment gets destroyed.   
    **If was set to false, it is up to the chat creator, to end the chat session. Should be done as follows:**   
    >  `chatController.endChat()`
  

