---
layout: default
title: Chat Settings
parent: Chat Configuration
nav_order: 2
# permalink: /docs/chat-configuration/chat-settings
---

# Chat settings {{site.data.vars.force-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
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
---

## Available configurations
{: .mt-6}
### Voice control
**_voiceSettings_** - Defines the desired voice feature behavior.   
Voice behavior available for chat, will be configured according to this setting and the available voice behavior supported by the ChatHandler.   

Available voice behavior options are defined by <u>`VoiceSupport`</u>.   
>**None, SpeechRecognition, VoiceToVoice, HandsFreeVoiceToVoice<sub>(currently not supported)</sub>**

{: .mt-8}
### Chat transcript request
Each supported chat is being "recorded" while in progress, to allow passing its transcript, if requested, just before it ends or being escalated.
e.g., Passing AI chat flow to a live agent, when AI chat is being escalated to a live or messaging chat.
In order to change and control which chat scope can request the preceding chat records, use the following:
```kotlin
val conversationSettings = ConversationSettings()
                           .enableChatLogRequest(Requesting_Scope, isEnabled)
```

> e.g., In order to enable passing preceding bot chat records to the live agent:
```kotlin
ConversationSettings().enableChatLogRequest(StatementScope.BoldScope, true)         
```   

- By default, `BoldScope` chat is enabled to request preceding chats. 

{: .mt-8}   
### Features availability configurations
Enable/Disable/Inspect support of some of the SDKs provided features.
- Autocomplete - Configure and Inspect support for AI chats

- Feedback - Inspect configured support for AI chats

- Typing indication - configure display for live chats

- Chatbar top header - configure display for live chats

- End chat over chat header - configure display

- Chat scroller display - scroll to bottom floating button

- Datestamp header - configure display and format


