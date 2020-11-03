---
layout: default
title: AccountInfoProvider
nav_exclude: true
---

# AccountInfoProvider
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Overview
Provides the hosting app to set chat session data and configurations, for chat creation, and be notified about chat session updates.
{: .overview}

Implementation should be passed on `ChatController` creation.
```kotlin
val chatController = ChatController.Builder(context)
                                .accountProvider(accountInfoProvider)
                                ...
                                .build(account,...)
```
---

## Why do i need to implement

<a id="account-provide"/>

- ### Set data for chat creation    
    **To be able to set data and configurations for chat session creation, when <u>escalating from an ai chat</u>**.   
    **`AccountInfoProvider.provide`** will be called, with a "starting point" Account. This account is configured with the details provided by the chat channel (like: apikey and appId). The App can set whatever is needs for the chat session before it passes the updated account on the callback. 
    ```kotlin
    class SimpleAccountProvider : AccountInfoProvider {

        var accounts: MutableMap<String, AccountInfo> = mutableMapOf()

        override fun provide(account: AccountInfo, callback: Completion<AccountInfo>) {
            accounts[account.getApiKey()]?.let{
                account.getInfo().update(it.getInfo())
                // update session related data for the upcoming chat
                account
            }?:let{
                addAccount(account)
            }
            callback.onComplete(account)
        }
    }
    ```

<a id="account-update"/>
- ### Notified about chat session data
    **To be able to receive account session data updates. like: _chatId_, _SenderId_, _visitorId_, etc.**   
    **`AccountIfoProvider.update'** will be called with an updated account. The updated values (or the whole account) can be saved for later use.
  ```kotlin
  class SimpleAccountProvider : AccountInfoProvider {

    var accounts: MutableMap<String, AccountInfo> = mutableMapOf()

    override fun update(account: AccountInfo) {
        accounts[account.getApiKey()]?.getInfo()?.update(account.getInfo())
    }
  }
  ```
---

<a id="session-info"/> 

## Setting chat session configurations
_Each account has a `SessionInfo` property. With this property you can provide extra data <sub>(see `SessionInfoKeys`)</sub> and configurations <sub>(see `SessionInfoConfigKeys`)</sub>, that will be used in chat session creation._

```kotlin
Kotlin samples:

// set extraData for [Live chat]:
boldAccount.info.addExtraData(SessionInfoKeys.FirstName to firstName,
                SessionInfoKeys.LastName to lastName,
                SessionInfoKeys.Email to email,
                SessionInfoKeys.Phone to phoneNumber, ...)

// Set live chat session to skip the prechat form: 
boldAccount.info.skipPrechat = true

// set user data for [Messaging chat]:
asyncAccount.info.UserInfo = UserInfo(UserId).apply{
                                firstName = "firstname"
                                lastName = "lastname"
                                email = "email"
                                phoneNumber = "phone number" 
                                countryAbbrev = “<country>”
                                profilePic = “URL”
                             }

// set lastReceievdMessageId for [Messaging chat]:
asynAccount.info.LastReceivedMessageId = ID               

// set SenderId for [Messaging chat]:
asynAccount.info.SenderId = ID 

// set Bot chat welcome message id (override id configured in the Console):
botAccount.info.welcomeMessage = articleId

////////////////////////////

java samples:

boldAccount.getInfo().addExtraData(new Pair(SessionInfoKeys.FirstName,firstName),...)

LiveSession.setLanguage(boldAccount.getInfo(), "fr-FR")

AsyncSession.setUserInfo(asyncAccount.getInfo(), new UserInfo(userId))
```

--- 

## Ongoing session configurations updates
Some configurations may get updated while the chat is in progress.   
As in **Messaging chat**, `LastReceivedMessageId' is updated on every agent incoming message.   
We provide a way for the embedding App, to be notified of these changes and be able to persist the most recent data.   

Instead of `AccountInfoProvider`, pass an implementation of `AccountSessionListener` to the ChatController.
Updates of ongoing configuration changes are passed over `AccountSessionListener::onConfigUpdate` method.

```kotlin
// call from SDK example:
accountListener?.onConfigUpdate(account, SessionInfoConfigKeys.LastReceivedMessageId, id)
```

```kotlin
class MySessionListener : AccountSessionListener {
    ...
    override fun onConfigUpdate(account: AccountInfo, updateKey: String, updatedValue: Any?) {
        val savedAccount = ... // fetch the relevant account record
        savedAccount.info
            .addConfigurations(updateKey to updatedValue) // update the account record

        // Or use what ever mechanism you have to save specific account session data ...
    }
}

val chatController = ChatController.Builder(context)
                                .accountProvider(mySessionListener)
                                ...
                                .build(account,...)
```