## Async chat continuation
### What is needed to enable chat continuation: 
- #### UserInfo - 
  Provide a `UserInfo` (userId), in case the current chat should relate to a specific user.
  All related chats are gathered as one conversation.   
  > If UserInfo (userId) was not provided with the account, a new UserInfo with auto generated userId will be created on chat start. 
  ```kotlin
  // sets a userInfo to the async session
  asyncAccount.info.UserInfo = UserInfo(UserId).apply{ ... }
  ```

- #### ShouldStartNewChat -
  Defines whether the created chat session should be restored on a previously opened chat or create a new chat session.
  ```kotlin
  asyncAccount.info.ShouldStartNewChat = true/false
  ```

- #### SenderId - 
  SenderId represents a chat session id. User can have many chat sessions in which accumulate to a user conversation.   
  SenderId functionality depends on [`ShouldStartNewChat`](ShouldStartNewChat) value. 
  ```kotlin
  asyncAccount.info.SenderId = Long_Sender_Value
  ``` 
  Provide a `SenderId` in order to:
   - restore connection to an **_active_** chat. [`ShouldStartNewChat==false`]
     > Chat restore will fail if the chat session was already been closed by agent.    
     A new chat session will be created, with newly generated SenderId. Missed messages of the closed chat will be fetched and displayed.
   - Enable retrieval of missed messages from a previously opened/closed chat. [`ShouldStartNewChat==true`]  
     >For that purpose, you'll also need to provide the [`lastReceivedMessageId`](listening_to_lastReceivedMessageId_changes) 
     ```kotlin
     asyncAccount.info.LastReceivedMessageId = Last_Message_Id
     ``` 
  
### How to get continuation values:

When chat is being created, or when values were changed, an update call will be triggered over [`AccountInfoProvider`](android-AccountInfoProvider).   
Save session details for later usage.

### listening to `lastReceivedMessageId` changes
As mentioned above, previous chats messages can't be retrieved without providing the `lastReceivedMessageId`. 
In order to be updated of id changes, implement `AccountSessionListener` instead of `AccountInfoProvider` and pass it to the ChatController builder instead.
```kotlin
val chatController = ChatController.Builder(context)
                                .accountProvider(accountSessionListener)
                                ...
                                .build(account,...)
```
Updates of ongoing configuration changes are passed over `AccountSessionListener::onConfigUpdate` method.
```kotlin
// call from SDK example:
accountListener?.onConfigUpdate(account, SessionInfoConfigKeys.LastReceivedMessageId, id)

// implementation example:
class MySessionListener : AccountSessionListener {
    ...
   
    fun onConfigUpdate(account: AccountInfo, @SessionInfoConfigKeys updateKey: String, updatedValue: Any?) {
            // save config value on your saved account
            // account param is just for reference.    
            // for example:
            val savedAccount: AccountInfo = getSavedAccount(...)
            savedAccount?.getInfo()?.addConfigurations(Pair<Any?, Any?>(updateKey, updatedValue))  
    }
}
```
