---
layout: default
title: Conversation Settings
nav_exclude: true
---

# Conversation Settings
{: .no_toc }

need work
{: .btn .btn-orange .fs-3 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
Chat features are configured by the various consoles available for every chat type, and by the SDKs pre defined available options.  
Chat configurations can be controlled by the embbeding App, by passing a customed `ConversatioSettings` object on ChatController creation.
{: .overview}

```kotlin
val settings = ConversationSettings()
    // do some customed change

val chatController = ChatController.Builder(context).apply {
                        conversationSettings(settings)
                        ....
                     }.build(account...)
```

## Available configurations

- ### Voice control
   **_voiceSettings_** - Defines the desired voice feature behavior.   
   Voice behavior available for chat, will be configured according to this setting and the available voice behavior supported by the ChatHandler.   

   Available voice behavior options are defined by <u>`VoiceSupport`</u>.   
   >**None, SpeechRecognition, VoiceToVoice, HandsFreeVoiceToVoice<sub>(currently not supported)</sub>**


- ### Enable/Disable chat transcript request
   In order to change and control which chat scope can request the preceding chat records, use the following:
   ```kotlin
   val conversationSettings = ConversationSettings()
                              .enableChatLogRequest(Requesting_Scope, isEnabled)
   ```

   > Exp: In order to enable passing preceding bot chat records to the live agent:
   ```kotlin
   ConversationSettings().enableChatLogRequest(StatementScope.BoldScope, true)         
   ```   
   
   - By default, `BoldScope` chat is enabled to request preceding chats. 