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
```