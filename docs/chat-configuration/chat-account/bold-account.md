---
layout: default
title: BoldAccount
parent: Setting Account
grand_parent: Chat Configuration
nav_order: 2
permalink: /docs/chat-configuration/setting-account/bold-account/
---

# BoldAccount
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


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