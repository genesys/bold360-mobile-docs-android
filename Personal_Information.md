# App provided information

The App can provide information on the specific user to server as per request (during run time).

There are 2 kinds of info to provided:

1. `Missing entites` and `Personal information`.
   The App needs to register its supplied entites.
   The provider that is contained via the article request a specific entity.
   If the requested entitie had been registered by the App, the `EntitesProvider` would be called to provide the info.

   ![provide missing entites / personal info](images/Android/personalInfo.png)

2. `initialization entites` - are global entities to be provided to the whole session.

## **How to use (using example)**

## Missing entities - Article example for balance scenario

the article body is: 
`The balance of your account ending with [[ACCOUNT.ID]] is [[ACCOUNT.CURRENCY]] {{getAccountBalance([[ACCOUNT.ID]],[[ACCOUNT.TYPE]])}} [[=ACCOUNT_OPTIONS]]`

- NOTE: The provider itself is not provided for this example.
        In order to get additional support please contact our support team.

### Init the BotAccount

```java
    BotAccount account = new BotAccount();
    account.setAccount("ACCOUNT");
    account.setApiKey("API KEY");
    account.setKnowledgeBase("KNOWLEDGEBASE");

    // The registeration of the missing entites that match the provider:
    account.setEntities(new String[]{"REQUESTED_BALANCE_ENTITIY", "REQUESTED_ACCOUNT"});
```

### In order to provide personal information and entities according to the SDK provider's requests

1. Implement the `EntitiesProvider` interface.

- When **personal information** is requested by the SDK, it should be provided by the next `provide` method:

```java
public void provide(@NotNull PersonalInfoRequest personalInfoRequest, @NotNull PersonalInfoRequest.Callback callback)
```

- When **missing entities** are requested by the SDK, it should be provided by the next `provide`
method:

```java
public void provide(@NonNull ArrayList<String> entities, @NonNull Completion<ArrayList<Entity>>
onReady)
```

2. Pass the implementation of the `EntitiesProvider` to the `ChatController` at the build.

```kotlin
   ChatController.Builder(getContext()).entitiesProvider(EntitiesProviderImpl)...build(...)
```


### The `EntitiesProvider` interface Implementation example

```kotlin
class BalanceEntitiesProvider : EntitiesProvider {

    private val random = Random()

    // Personal Info:
    override fun provide(personalInfoRequest: PersonalInfoRequest, callback: PersonalInfoRequest.Callback) {

        when (personalInfoRequest.id) {

            "getAccountBalance" -> {
                val balance = (random.nextInt(10000)).toString()
                callback.onInfoReady(balance, null)
            }

            "getExpiration" -> callback.onInfoReady("01/2025", null)

            else -> callback.onInfoReady("1,000$", null)

        }
    }

    // Missing Entites:
    override fun provide(entities: ArrayList<String>, onReady: Completion<ArrayList<Entity>>) {
        val missingEntities = NRConversationMissingEntities()

        for (missingEntity in entities) {
               missingEntities.addEntity(createEntity(missingEntity))
        }

        (missingEntities.entities as? ArrayList<Entity>)?.apply { onReady.onComplete(this) }
    }

    private fun createEntity(entityName: String): Entity? {
        return when (entityName) {
            "REQUESTED_BALANCE_ENTITIY" -> {
                Entity(Entity.PERSISTENT, Entity.NUMBER, (random.nextInt(100 - 10) + 10).toString(), entityName, "1")
            }
            "REQUESTED_ACCOUNT" -> {
                Entity(Entity.PERSISTENT, Entity.NUMBER, "123", entityName, "1").apply {
                    addProperty(Property(Entity.TEXT, (random.nextInt(10000 - 1000) + 1000).toString(), "1").apply { name = "ID" })
                    addProperty(Property(Entity.TEXT, "$", "2").apply { name = "CURRENCY" })
                    addProperty(Property(Entity.TEXT, "PRIVATE", "2").apply { name = "TYPE" })
                }
            }
            else -> null
        }
    }
}
```

## Initialization Entities
If the account is using Initialization Entities, create BotAccount as follows:

### Create the initialization entities map

```kotlin
        val entities = mapOf("EntityKey1" to "EntityValue1",
                            "EntityKey2" to "EntityValue2",
                            ... )
```

### Init the BotAccount
```kotlin

        val account = BotAccount(API_KEY, ACCOUNT_NAME, KNOWLEDGE_BASE,
                                  SERVER, CONTEXT_MAP).apply {
                                      initializationEntities = entities
                                  }
```

- Note: The `initialization entities` can be also supplied at the `ChatEventListener`'s OnAccountReady method as follows:

```kotlin
    override fun onAccountReady(account: Account, settings: MutableMap<String, Any>) {
        ...
        (account as? BotAccount)?.initializationEntities = hashMapOf(Pair("USERID", "12345"))
        ...
    }
```
