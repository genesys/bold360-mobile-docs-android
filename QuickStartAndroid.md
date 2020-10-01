# Using the Android SDK

## Starting a Chat  
Do the following
1. ### <u>Create an Account.</u>

- #### <u>*To start chat with Bot create `BotAccount`*:</u>  

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

    - If the welcome message should be customised and override current console configurations, create account as follows:

        ```kotlin
        val account = BotAccount(API_KEY, ACCOUNT_NAME, KNOWLEDGE_BASE,
                                SERVER, CONTEXT_MAP).apply {
                                    welcomeMessage = ARTICLE_ID
                                }
        ```

    - If the account is using Initialization Entities, please follow this [link](./Personal_Information.md#Initialization_entites)

- #### <u>*To start chat with Bold create `BoldAccount`*:</u>
    - extraData - Detailes about the user and the current chat session. The `extraData` details will be used to fill the prechat form if enabled, and will provide the agent some details about the user.

    - securedInfo - An encrypted secured string that was applied to the specific access key in order to validate the chat origin on creation.

    ```kotlin
    val account = BoldAccount(API_KEY)

    
    account.apply{
        // adding extraData: 
        addExtraData (SessionInfoKeys.Department to BOLD_DEPARTMENT,
            SessionInfoKeys.FirstName to DemoFirstName,
            SessionInfoKeys.LastName to DemoLastName
            ...)             

        // adding secured info:
        info.securedInfo = "..."    
    }
    ```
    

- #### <u>*To start a Messaging chat create `AsyncAccount`*:</u>
    
    ```kotlin
    val account = AsyncAccount(API_KEY, APPLICATION_ID)
    ```
    
    In order to provide a specific user id and info, (by that, relate all chats with the same id, to the same user), add `UserInfo` to the account creation. If `userId` is not provided, one will be automatically generated. 
    
    ```kotlin
    // kotlin:
    val account = AsyncAccount(API_KEY, APPLICATION_ID).apply {
        info.userInfo = UserInfo(USER_ID).apply { // Mandatory
            firstName = FIRST_NAME // optional
            lastName = LAST_NAME // optional
            email = EMAIL_ADDRESS // optional
            phoneNumber = PHONE_NUMBER // optional
        }
    }
    ```
    ```java
    // Java:
    UserInfo userInfo = new UserInfo(USER_ID);
    userInfo.setFirstName(FIRST_NAME);
    userInfo.setLastName(LAST_NAME);
    userInfo.setEmail(EMAIL_ADDRESS);
    userInfo.setPhoneNumber(PHONE_NUMBER);

    AsyncAccount account = new AsyncAccount(API_KEY, APPLICATION_ID);
    AsyncSession.setUserInfo(account.getInfo(), userInfo)
    ```
    > Async messages response wait timeout can be configured on the accounts SessionInfo as well:
    `info.ackTimeout = MS_Value`. If was not set,the default value will be used, 6000ms. 

##
2. ### <u>Create a ChatController object</u>
    With the ChatController one can create and control multiple chats.
    The chat type is configured by the Account that is provided on chat creation.

    ```kotlin
    val chatController = ChatController.Builder(context)
                                          .build(account, ...)

    ...
    // start a new chat, using same chatController:
    chatController.startChat(account)

    // restore active chat or starts new chat
    chatController.restoreChat(fragment?, account?)
    ```

##

3. ### <u>Add the chat fragment to your activity.</u>

    Implement the ChatLoadedListener interface and pass it in the `ChatController.Builder` build method.   
    Once the chat build succeeded and the fragment is ready to be displayed, `onComplete` will be called with the fragment on the result. 

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
##

> **Notice - Make sure to activate `ChatController.destruct()`, when the ChatController instance is no longer needed** (exp: Chats section in app
is being closed or the ChatController instance is being replaced). Destruct will verify resources release (including open sockets and communication channels).


---

### Code Sample
[bold360ai samples](https://github.com/bold360ai/bold360-mobile-samples-android)
