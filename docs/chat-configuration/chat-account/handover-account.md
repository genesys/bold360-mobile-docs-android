---
layout: default
title: HandoverAccount
parent: Setting Account
grand_parent: Chat Configuration
permalink: /docs/chat-configuration/setting-account/handover-account/
nav_order: 4
---

# HandoverAccount

Use this account to create chats with third party live chat providers. Usually used for [Handover chats](/docs/advanced-topics/handover-chat). 

This account is being created automatically by the SDK when chat escalation to `Handover` chat, was activated on chat with AI.
  
```kotlin
val chatConfig = "provider defined configuration string"
val account = HandoverAccount(chatConfig)
```    