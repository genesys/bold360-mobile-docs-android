# Using the Android SDK

## Starting a Chat  
Do the following
1. ### <u>Create an Account.</u>

   - #### *To start chat with Bot create `BotAccount`*:  
    
      ```kotlin
      val account = BotAccount(API_KEY, ACCOUNT_NAME,
                              KNOWLEDGE_BASE, SERVER, CONTEXT_MAP)
      ```  
  
      Where: API_KEY (mandatory), ACCOUNT_NAME(mandatory), KNOWLEDGE_BASE(mandatory), SERVER(mandatory), CONTEXT_MAP(optional)

      - If the account is using Context, create BotAccount as follows:

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
                                       welcomeMessage = ARTICLE_ID
                                  }
        ```


   - #### *To start chat with Bold create `BoldAccount`*:

        ```kotlin
        val account = BoldAccount(API_KEY)
        ```


    - #### *To start an async chat create `AsyncAccount`*:
     
        ```kotlin
        val account = AsyncAccount(API_KEY, APPLICATION_ID)
        ```
        
        In order to provide a specific user id and info, (by that, relate all chats with the same id, to the same user), add `UserInfo` to the account creation. If `userId` is not provided, one will be automatically generated. 
        
        ```kotlin
        val account = AsyncAccount(API_KEY, APPLICATION_ID).apply {
            info.userInfo = UserInfo(USER_ID).apply { // Mandatory
                firstName = FIRST_NAME // optional
                lastName = LAST_NAME // optional
                email = EMAIL_ADDRESS // optional
                phoneNumber = PHONE_NUMBER // optional
            }
        }
        ```

##

2. ### <u>Create `ChatController` object</u>
    With the ChatController one can create and control a chat.

    ```kotlin
    val chatController = ChatController.Builder(context)
                                          .build(account, ...)
    ```

##

3. ### <u>Add the chat fragment to your activity.</u>

    Implement the ChatLoadedListener interface and pass it in the `ChatController.Builder` build method.   
    Once the chat build succeeded and the fragment is ready to be displayed, `onComplete` will be called and the fragment will bre available on the results. 

    ```kotlin
    ChatController.Builder(context).build(account, object : ChatLoadedListener {
            override fun onComplete(result: ChatLoadResponse) {
                result.error?.run{
                  // report Chat load failed
                } ?: run{
                  // add result.getFragment() to the applications activity.
                }
            }
        })
    ```

---

### Code Sample
[bold360ai samples](https://github.com/bold360ai/bold360-mobile-samples-android)
