# Chat restoring and continuation
## Chat restoring
A key player in chat restoring is the `ChatController`.
### ChatController
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


### Create and restore chats
- #### Create chat using the ChatController.Builder
  Creates a new `ChatController` and a new chat, with the provided account.
  ```kotlin
  val chatController = ChatController.Builder(context).apply {
                            conversationSettings(...)
                            chatEventListener(...)
                            ...
                        }.build(account, ChatLoadedListener{})
  ```

- #### Create and restore chats using `ChatController` instance with a provided account 
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
  
---

## Chat continuation
The option to continue previously created chat later in time.  
Each chat type (bot, live) has its own properties that are being used to enable that option.   
Those properties are configurable via the chat account.  

#### Session data updates
In order to get updates of chat session data, implement [`AccountInfoProvider`](./android-AccountInfoProvider) and pass it on `ChatController` creation. 
```kotlin
val chatController = ChatController.Builder(context).apply{
                        accountProvider(MyProvider)
                     }               
                    .build(account, ...)
```

### Async chat continuation
The values that are needed to enable chat continuation are: 
- #### UserInfo - 
  Provide a `UserInfo` (userId), in case the current chat should relate to a specific user.
  All related chats are gathered as one conversation.   
  > If UserInfo (userId) was not provided with the account, a new UserInfo with auto generated userId will be created on chat start. 
  ```kotlin
  // sets a userInfo to the async session
  asyncAccount.info.UserInfo = UserInfo(UserId).apply{ ... }
  ```

- #### SenderId - 
  Provide a `SenderId` in order to restore connection to an **_active_** chat.   
  > If chat was closed by the time of the connection, a new chat session will be created, new SenderId will be generated and missed messages if available, will be fetched and displayed.
  ```kotlin
  // sets SenderId to restore async session connection to a specific chat:
  asyncAccount.info.SenderId = Long_Sender_Value
  ```

- #### PreviousSenderId -
  Provide a `PreviousSenderId` in order to fetch messages of a previous closed chat.   
  > **_For that purpose, you'll also need to provide the `lastReceivedMessageId`_** 
  ```kotlin
  // sets PreviousSenderId to enable fetching missed messages of a previous closed chat. 
  asyncAccount.info.PreviousSenderId = Long_Previous_Sender_Value

  // followed by:
  asyncAccount.info.LastReceivedMessageId = Last_Message_Id
  ```

How to get continuation values:

While chat is being created and started, an updated values of this 3 values will be sent to the integrating app, for later use.
This is done via the [`AccountInfoProvider`](android-AccountInfoProvider) update method.

#### listening to `lastReceivedMessageId` changes
As mentioned above, previous chats messages can't be retreived without providing the `lastReceivedMessageId`. 