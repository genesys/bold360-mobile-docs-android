# Using the Android SDK

## Starting a Chat  
### 1. Create an Account.

  > To start chat with Bot create `BotAccount`:  
    
  ```kotlin
  val account = BotAccount(API_KEY, ACCOUNT_NAME,
                           KNOWLEDGE_BASE, SERVER, CONTEXT_MAP)
  ```  
  
Where: API_KEY (mandatory), ACCOUNT_NAME(mandatory), KNOWLEDGE_BASE(mandatory), SERVER(mandatory), CONTEXT_MAP(optional)

  - If the account is using Context, create account as follows:

    ```kotlin
    val contexts = mapOf("ContextKey1" to "ContextValue1",
                        "ContextKey2" to "ContextValue2",
                        ... )

    val account = BotAccount(API_KEY, ACCOUNT_NAME,
                          KNOWLEDGE_BASE, SERVER, contexts)
    ```
    
  - If the account is using Initialization Entities, create BotAccount as follows:

    ```kotlin
    val entities = mapOf("EntityKey1" to "EntityValue1",
                        "EntityKey2" to "EntityValue2",
                        ... )

    val account = BotAccount(API_KEY, ACCOUNT_NAME, KNOWLEDGE_BASE,
                                SERVER, CONTEXT_MAP).apply {
                                    initializationEntities = entities
                                }
    ```

  - If the welcome message should be customised and override current console configurations, create account as follows:

    ```kotlin
    val account = BotAccount(API_KEY, ACCOUNT_NAME, KNOWLEDGE_BASE,
                              SERVER, CONTEXT_MAP).apply {
                                  welcomeMessage = "64785388"
                              }
    ```

To start chat with Bold create `BoldAccount`:

  ```kotlin
  val account = BoldAccount(API_KEY)
  ```

---

### 2. Create a `ChatController`
With a ChatController one can create and control a chat.

```kotlin
val chatController = ChatController.Builder(context)                                                     
                                     .build(account, ...)
```

---

### 3. Add chat fragment to the application.

In order to add the chat fragment to the application's activity, implement the ChatLoadedListener interface and pass it in the `ChatController.Builder` build method.

```kotlin
ChatController.Builder(context).build(account, object : ChatLoadedListener {
        override fun onComplete(result: ChatLoadResponse) {
            if (result.getError() != null) {
                // in case something went wrong with chat load
            } else {
              // Chat fragment (result.getFragment()) is now available on the result and be added to the applications activity.
            }
        }
    })
```
