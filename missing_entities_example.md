# Missing entities - Article example for balance scenario

The example article body is:
`The balance of your account ending with [[ACCOUNT.ID]] is [[ACCOUNT.CURRENCY]] {{getAccountBalance([[ACCOUNT.ID]],[[ACCOUNT.TYPE]])}} [[=ACCOUNT_OPTIONS]]`

- NOTE: The provider itself is not provided for this example.
        In order to get additional support please contact our support team.

## Configure the BotAccount

```kotlin
    BotAccount account =  BotAccount(apiKey, accountName, kb, server).apply {
        // The registeration of the missing entites that match the provider:
        entities = arrayOf("REQUESTED_BALANCE_ENTITIY", "REQUESTED_ACCOUNT")
    }
```

## The `EntitiesProvider` interface Implementation

```kotlin
class BalanceEntitiesProvider : EntitiesProvider {

    private val random = Random()

    // Personal Information:
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
