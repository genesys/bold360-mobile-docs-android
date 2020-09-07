# Account entities and personal information

The SDK provides the bridge to pass user specific information to the BOT on chat start and while processing responses to user queries, which demands more details from the user, during the chat.

There are 2 types of personal information

### __Initialization entites__

Predefined data values that can be provided for the whole chat session and are not being changed dynamically (exp: ids, keys, etc).

#### How To use

1. Create the initialization entities map

   ```kotlin
   val entities = mapOf("EntityKey1" to "EntityValue1", "EntityKey2" to "EntityValue2", ... )
   ```

2. Set the initialization entities on the account

   ```kotlin
   val account = BotAccount(...).apply {
                             initializationEntities = entities
                        }
   ```

- Note: The `initialization entities` can be also supplied at the `AccountInfoProvider.provide` method as follows:

    ```kotlin
    override fun provide(account: AccountInfo, callback: Completion<AccountInfo>) {
        ...
        (account as? BotAccount)?.initializationEntities = hashMapOf(Pair("USERID", "12345"))
        ...
    }
    ```

### __Missing entites and Personal information__

Dynamically required details. Depends on user query and the article the Bot responses with.
articles of this sort are configured with a Bold360ai provider which formats the response to contain the entities tag pattern, that are being recognized by the SDK as the needed information (please contect out support for farther information about the [Bold360AI provider](https://support.bold360.com/bold360/help/how-do-i-create-a-csv-provider)).

- **Missing entites**:    
  Details that the user is asked to provide before the requested data can be supplied.   
  The provider which assigned to the article identifies which extra details are needed in order to get a response to a user query. According to the entities that were set to the account, the App is asked to provide thoes details by calling the `EntitiesProvider.provide`.   
  ##### Missing entities can be, names, account numbers, etc.

- **Personal information**:   
  Extra user personal info, that may be requested from the App by the SDK regarding a specific entity, in order to provide a response.
  Needed once the BE provides a response which contains, info needed place holders. The App provided personal info is than used to replace those place holders.
  ##### Personal information can be, account balance, passport number, etc.

##  
  ![provide missing entites / personal info](images/Android/personalInfo.png)
##

#### How To use

1. Create the missing entities array

    ```kotlin
    val missingEntities = arrayOf("EntityKey1", "EntityKey2"... )
    ```

2. Set the entities on the account

     ```kotlin
    val account = BotAccount(...).apply {
                              entities = missingEntities
                         }
    ```

3. Implement the `EntitiesProvider` interface.
   At the `EntitiesProvider` there are two `provide` methods for `personal information` and for `missing entities`.

4. Pass the implementation of the `EntitiesProvider` to the `ChatController` at the ChatController creation

    ```kotlin
    ChatController.Builder(getContext()).entitiesProvider(the EntitiesProvider implemintation)...build(...)
    ```

 > ##### [Full example: `missing entities` and `personal information` usage](missing_entities_example.md)
