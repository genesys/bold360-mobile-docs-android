# Tracking user activity
In order to track user activity during chat session, the SDK gets a unique `userId` per chat session. This id is passed on all API requests.

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
