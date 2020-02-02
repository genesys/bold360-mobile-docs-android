## Async chat continuation
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

### How to get continuation values:

When chat was created, or when values were changed, an update call will be triggered over [`AccountInfoProvider`](android-AccountInfoProvider).   
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
