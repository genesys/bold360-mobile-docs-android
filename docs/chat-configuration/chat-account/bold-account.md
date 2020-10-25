---
layout: default
title: Bold Live Chat
parent: Chat Account
grand_parent: Chat Configuration
nav_order: 2
---

# Bold Live Chat - chat with live agent {{site.data.vars.force-work}}
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Overview
...


## BoldAccount
Use this account to create synced live chat sessions with an agent.

### Configure chat session
Chat session and details can be configured by the account.   
User details can be provided for the chat session, and will be available for the receiving agent to view.     

- **extraData** - Detailes about the user and the current chat session. The `extraData` details will be used to fill the prechat form if enabled, and will provide the agent some details about the user.

- **securedInfo** - An encrypted secured string that was applied to the specific access key in order to validate the chat origin on creation.

- **VisitorId** - if provided, the created chat will be added to the same user account chats history. The same id will be used on the new chat.

```kotlin
val account = BoldAccount(API_KEY)

account.apply{
    // adding extraData: 
    addExtraData (SessionInfoKeys.Department to BOLD_DEPARTMENT,
        SessionInfoKeys.FirstName to DemoFirstName,
        SessionInfoKeys.LastName to DemoLastName
        ...)             

    // adding secured info, in case of validated chats creation:
    info.securedInfo = "..."  

    // Create a chat that relates to an existing user:
    info.id = VISITOR_ID
    //OR
    info.visitorId = VISITOR_ID  
}
```
---

### How to

- Define chat branding strings language.   
`account.info.language = "FR-fr"`

- Define the id of a specific department that should receive the chat.    
`account.info.department = DEPARTMENT_ID`

- Create a chat without displaying the prechat form.
`account.info.skipPrechat = true` or `account.skipPrechat()`   
  ##### _Notice: If the prechat form is being skipped, you can still send user details via extraData._
  {: .no_toc}

---

### Account updates
Once the chat session was created, an account update will be triggered on [AccountInfoProvider](/docs/chat-configuration/setting-account/account-info-provider) implementation.  
From the updated account you can fetch the `ChatId`.  
`account.info.chatId` 

---


## Starting a live chat
{: .d-inline-block}
Live chat may start as main chat purpose, or be triggered from chat with BOT.

## Starting chat with Bold live
    A Bold live account should be provided over `ChatController` creation as follows:
    ```kotlin
    val account = BoldAccount(API_KEY)
    val chatController = ChatController.Builder(context)                                                     
                                        .build(account, ...)
    ```
    ![](images/Android/start_with_live.png)

## Starting chat with Bot and continue to Bold live
    - A chat typed channel should be configured on the Bold360ai console.<sup>[(*)](https://support.nanorep.com/API-Integrations/Chat-Integration/1009694282/How-to-integrate-LiveChat-Inc-chat.htm)</sup>.   
    - Bold live chat can than be triggered by user selection of that channel.    
    - App will be asked to `provide` (over `AccountInfoProvider`), the BoldAccount that matches the details provided by that channel. (If `AccountInfoProvider` implementation was not provided, the chat will start with default configurations.)   
    
    ![](images/Android/bot_to_live.png)


### Listening to account updates
In order to get account info updates, like visitoId, chatId and other details that were filled by the user on the prechat, an implementation of `AccountInfoProvider` should be set on `Chatcontroller.Builder` 
```kotlin
val chatController = ChatController.Builder(context) 
                    .accountProvider(accountInfoProviderImpl)
                    ...
                    .build(account,...)
```

- When the SDK asks for account from the app, (as in live chat start from bot chat), the `provide` method will be activated.
- When account's info was updated, as when chat created and prechat submitted, the `update` method will be activated.

```kotlin
class MyAccountProviderImpl : AccountInfoProvider{
    override fun provide(accountInfo: AccountInfo, callback:    Completion<AccountInfo>) {
        // get app saved account matching the parameter one [accountInfo.getApiKey()]
        // pass the account to the SDK over the callback:
        callback.onComplete(app_account)
    }

    override fun update(account: AccountInfo) {
        // get app saved account matching the parameter one [accountInfo.getApiKey()]
        // update the account:
        app_account.update(account) // inner info will be updated    
    }
}
```

### How can i access current chat info
While a live chat goes through the chat lifecycle, chat info will be updated with some ids and details. In order to see those details, access the accounts `info` property.

- How to get the VisitorId:   

        kotlin -> account.info.visitorId 
                    OR 
                  account.info.id
        
        java   -> getVisitorId(account.getInfo()) 
                    OR 
                  account.getInfo().getId()


- How to get the chatId:

        kotlin -> acount.info.chatId
        
        java   -> getChatId(account.getInfo())

- How to skip prechat:

        kotlin -> boldAccount.skipPrechat()
                    OR
                  account.info.skipPrachat(true)

        java   -> boldAccount.skipPrechat()
                    OR
                  setSkipPrechat(account.getInfo(), true);
 

### How to configure the created chat
Some configurations may be applied over the account in order to define aspects of the created chat. Like langauge, prechat form fields values, **passing `visitorId` to enable continuation of previous chats**, etc.    
For that purpose we have the `extraData` on the account `info` member. (see `VisitorDataKeys` for the list of configurable data)

- How to set chat language:
  
      account.addExtraData(VisitorDataKeys.Language to "fr-FR")
  
- How to set a specific department: <sub>_**(needed when skipPrechat is enabled)**_</sub>
        
      account.addExtraData(VisitorDataKeys.Department to 2279019778245965656)
  
- How to set details for the prechat:
      
      account.addExtraData(VisitorDataKeys.FirstName to "Ando",
                            VisitorDataKeys.LastName to "Roid",
                            VisitorDataKeys.Email to "a@gmail.com",
                            ...)

- How to set the `visitorId`:   

      account.info.visitorId = ...
 

## Live Chat continuity
...