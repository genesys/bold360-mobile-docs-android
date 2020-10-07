---
layout: default
title: BoldAccount
parent: Setting Account
grand_parent: Chat Configuration
nav_order: 2
permalink: /docs/chat-configuration/setting-account/bold-account/
#has_toc: false
---

# BoldAccount

The account can be set with configurations and extraData, which will be used for chat creation.

- extraData - Detailes about the user and the current chat session. The `extraData` details will be used to fill the prechat form if enabled, and will provide the agent some details about the user.

- securedInfo - An encrypted secured string that was applied to the specific access key in order to validate the chat origin on creation.

- VisitorId - if provided, the created chat will be added to the same user account chats history. The same id will be used on the new chat.

```kotlin
val account = BoldAccount(API_KEY)

account.apply{
    // adding extraData: 
    addExtraData (SessionInfoKeys.Department to BOLD_DEPARTMENT,
        SessionInfoKeys.FirstName to DemoFirstName,
        SessionInfoKeys.LastName to DemoLastName
        ...)             

    // adding secured info:
    info.securedInfo = "..."    
}
```

