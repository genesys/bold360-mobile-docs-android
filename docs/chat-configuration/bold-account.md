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

if needed, add extraData values to the account. The `extraData` details will be used to fill the prechat form if enabled, and will provide the agent some details about the user.

```kotlin
val account = BoldAccount(API_KEY)

// adding extraData: 
account.apply{
    addExtraData (SessionInfoKeys.Department to BOLD_DEPARTMENT,
        SessionInfoKeys.FirstName to DemoFirstName,
        SessionInfoKeys.LastName to DemoLastName
        ...)             
}
```
