# Tracking user activity
In order to track user activity during chat session, the SDK generates by BE API a unique token, that should be used from this point forward for all chats sessions. This token enables tracking user activity.

## How to customize user id

Customizing `userId` is done by `BotAccount`.
```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                KNOWLEDGE_BASE, SERVER, CONTEXT_MAP).apply {
                        
                        userId(CUSTOMED_ID) // setting the user id
                } 
                
ChatController.Builder(context)...build(account, ...)

//OR

account.userId(CUSTOMED_ID)
```

In case the app doesn't customize the `userId`, the app needs to store the SDK's provided `userId`.
This can be done at the `update` function at the AccountSessionListener.
Find more in [AccountSessionListener](./android-AccountInfoProvider.md)
The app also needs to supply the stored userId as presented above.
