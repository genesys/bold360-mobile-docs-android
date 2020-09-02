# Account entities and personal information

The SDK provides the bridge to pass user specific information to the BOT on chat start and while processing responses to user queries, which demands more details from the user, during the chat.
User information can be provided in 2 ways:

## Initialization entites

Predefined data values that can be provided for the whole chat session and are not being changed dynamically (exp: ids, keys, etc).

### How To use

1. Create the initialization entities map

   ```kotlin
   val entities = mapOf("EntityKey1" to "EntityValue1", "EntityKey2" to "EntityValue2", ... )
   ```

2. Register your initialization entities as follows

   ```kotlin
   val account = BotAccount(...).apply {
                             initializationEntities = entities
                        }
   ```

- Note: The `initialization entities` can be also supplied at the `AccountInfoProvider`'s OnAccountReady method as follows:

    ```kotlin
    override fun provide(account: AccountInfo, callback: Completion<AccountInfo>) {
        ...
        (account as? BotAccount)?.initializationEntities = hashMapOf(Pair("USERID", "12345"))
        ...
    }
    ```

## Missing entites or Personal information

Dynamically required details. Depends on user query and the article the Bot responses with..
The article uses a Bold360AI provider implementation in order to provide the needed data (please contect out support for farther information about the Bold360AI provider).

- **Missing entites**: If the client (an App uses the mobile) registerered to the missing
  entities which are needed by the provider, the provider asks the client for the data
  The `EntitesProvider` would be called to provide the info (exp: names, account numbers, etc).

- **Personal information** - After the missing entitiy has been provided, the personal
  information about this missing entity can be also requested by the server  (exp: account balance, passport number, etc).

  ![provide missing entites / personal info](images/Android/personalInfo.png)

### How To use

1. Create the missing entities array

    ```kotlin
    val missingEntities = arrayOf("EntityKey1", "EntityKey2"... )
    ```

2. Register your dynamic entities as follows:

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

 >Follow the next [link](missing_entities_example.md) for a specific example of `missing entities` and `personal information` usage
