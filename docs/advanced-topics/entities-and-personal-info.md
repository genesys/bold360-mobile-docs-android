---
layout: default
title: Chat Entities and Personal information
parent: Advanced Topics
nav_order: 8
# permalink: /docs/advanced-topics/entities-and-personal-info
---

# Chat Entities and Personal information {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}

---

## Overview
Entities enable turning data into smart chatbot conversations. Entities essentially serve as a database for the information that the bot needs to answer users questions.   
The SDK provides the bridge to pass this data to the Chatbot on chat start and while processing responses to user queries.
{: .overview}

Entities may be configured as initialization entities or missing entities and personal information.

<a id="initentities"/>
## Initialization entities
Entities values that can be provided for the entire chat session and are not changed dynamically (e.g., ids, keys, etc).

### How To use

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

- Note: The `initialization entities` can be also supplied at the [`AccountInfoProvider.provide`]({{'/docs/chat-configuration/extra/account-info-provider#account-provide' | relative_url}}) method as follows:

    ```kotlin
    override fun provide(account: AccountInfo, callback: Completion<AccountInfo>) {
        ...
        (account as? BotAccount)?.initializationEntities = hashMapOf(Pair("USERID", "12345"))
        ...
    }
    ```

## Missing entities for personalized responses

These entities are use to dynamically retrieve details from the user, for personalizing the response to the user query.
Articles that are configured with entities, contains entities tags within the response which are recognized by the SDK as the missing information needed to properly display the article as intended. 
> Refer to [How to use Data Entities](https://support.bold360.com/bold360/help/how-to-use-data-entities) for more information.

- **Missing entites**:    
  Details that the user is asked to provide before the requested data can be supplied.   
  The provider which assigned to the article identifies which extra details are needed in order to get a response to a user query. According to the entities that were set to the account, the App is asked to provide thoes details by calling the `EntitiesProvider.provide`.   
  ##### Missing entities can be, names, account numbers, etc.

- **Personal information**:   
  Extra user personal info, that may be requested from the App by the SDK regarding a specific entity, in order to provide a response.
  Needed once the BE provides a response which contains, info needed place holders. The App provided personal info is than used to replace those place holders.
  ##### Personal information can be, account balance, passport number, etc.

--- 

  ![provide missing entites / personal info]({{'/assets/diagrams/personalInfo.png' | relative_url}})
  {: .image-70}

---

### How To use

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
---

 > view Sample: [`missing entities` and `personal information` usage]({{'/docs/faq/missing-entities-example' | relative_url}})
{: .mt-6}
