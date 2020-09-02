# Missing entities and Personal information example

Lets say the missing entity is `REQUESTED_ACCOUNT` which is the account entity that is needded for the article.

For this example, the article body is:

`The balance of your account ending with [[ACCOUNT.ID]] is [[ACCOUNT.CURRENCY]] {{getAccountBalance([[ACCOUNT.ID]],[[ACCOUNT.TYPE]])}}`

>NOTE: The provider itself is not provided for this example.
        In order to get additional support please contact our support team.

1. First lets register this entity to the account:

    ```kotlin
    BotAccount account =  BotAccount(apiKey, accountName, kb, server).apply {
        entities = arrayOf("REQUESTED_ACCOUNT")
    }
    ```

2. Missing entitiy creation

   This Entity would have a few properties:

    - ID
    - TYPE
    - CURRENCY

    So this Entity's implementation is:

    ```kotlin
    private val random = Random()

    val accountEntityExp = Entity(Entity.PERSISTENT, Entity.NUMBER, "123", entityName, "1").apply{
                            addProperty(Property(Entity.TEXT, (random.nextInt(10000 - 1000) + 1000).toString(), "1").apply { name = "ID" })
                            addProperty(Property(Entity.TEXT, "$", "2").apply { name = "CURRENCY" })
                            addProperty(Property(Entity.TEXT, "PRIVATE", "2").apply { name = "TYPE" })
                    }
    ```

3. Provide the personal information for the Missing Entity

   The requested private information for this entity is :

    ```kotlin
    val personalBalanceExp = (random.nextInt(10000)).toString()
    ```

4. The `EntitiesProvider` interface Implementation

    ```kotlin
    class BalanceEntitiesProvider : EntitiesProvider {

        private val random = Random()

        // Provide the Personal Information
        override fun provide(personalInfoRequest: PersonalInfoRequest, callback: PersonalInfoRequest.Callback) {

            when (personalInfoRequest.id) {

                "getAccountBalance" -> callback.onInfoReady(personalBalanceExp, null)

                else -> callback.onInfoReady("1,000$", null)

            }
        }

        // Provide the Missing Entites:
        override fun provide(entities: ArrayList<String>, onReady: Completion<ArrayList<Entity>>) {
            val missingEntities = NRConversationMissingEntities()

            for (missingEntity in entities) {
                if (entityName == "REQUESTED_ACCOUNT") {
                    missingEntities.addEntity(accountEntityExp)
                }  
            }

            (missingEntities.entities as? ArrayList<Entity>)?.apply { onReady.onComplete(this) }
        }
    }
    ```

5. Set the balance provider to the ChatController as explaned here [link](Personal_information.md)
