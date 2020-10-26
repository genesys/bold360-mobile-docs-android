---
layout: default
title: HandoverAccount
parent: Chat Account
grand_parent: Chat Configuration
# permalink: /docs/chat-configuration/chat-account/handover-account/
nav_order: 4
---

# HandoverAccount {{site.data.vars.need-work}}

Use this account to create chats with third party live chat providers. Usually used for [Handover chats]({{'/docs/advanced-topics/handover-chat' | relative_url}}). 

This account is being created automatically by the SDK when chat escalation to `Handover` chat, was activated on chat with AI.
  
```kotlin
val chatConfig = "provider defined configuration string"
val account = HandoverAccount(chatConfig)
```    