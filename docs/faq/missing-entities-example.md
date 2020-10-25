---
layout: default
title: Missing Entities Code Sample
parent: FAQ
nav_order: 3
# permalink: /docs/faq/missing-entities-example
---

# Missing entities and Personal information example {{site.data.vars.need-work}}
{: .no_toc}

Lets say the missing entity is `REQUESTED_ACCOUNT` which is the account entity that is needded for the article.

For this example, the article body is:

`The balance of your account ending with [[ACCOUNT.ID]] is [[ACCOUNT.CURRENCY]] 
{{getAccountBalance([[ACCOUNT.ID]],[[ACCOUNT.TYPE]])}}`

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

    val accountEntityExp = Entity(...).apply{
                            name = "Account"
                            addProperty(Property(...).apply { name = "ID" })
                            addProperty(Property(...).apply { name = "CURRENCY" })
                            addProperty(Property(...).apply { name = "TYPE" })
                    }
    ```

3. Provide the personal information for the Missing Entity

   The requested private information for this entity is :

    ```kotlin
    val personalBalanceExp = ...
    ```

4. The `EntitiesProvider` Implementation

    ```kotlin
    class BalanceEntitiesProvider : EntitiesProvider {

        // Provide the Personal Information
        override fun provide(personalInfoRequest: PersonalInfoRequest, callback: PersonalInfoRequest.Callback) {

            when (personalInfoRequest.id) {

                "getAccountBalance" -> callback.onInfoReady(personalBalanceExp, null)

                else -> callback.onInfoReady(..., null)

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

5. Set the balance provider to the ChatController:

    ```kotlin
    ChatController.Builder(getContext()).entitiesProvider(the EntitiesProvider implemintation)...build(...)
    ```
