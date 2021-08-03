---
layout: default
title: Chat Settings
parent: Chat Configuration
nav_order: 2
# permalink: /docs/chat-configuration/chat-settings
---

# Chat settings {{site.data.vars.need-work}}
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
{: .mb-4}
**_voiceSettings_** - Defines the desired voice feature behavior.   
Voice behavior available for chat, will be configured according to this setting and the available voice behavior supported by the ChatHandler.   

Available voice behavior options are defined by <u>`VoiceSupport`</u>.   
>**None, SpeechRecognition, VoiceToVoice, HandsFreeVoiceToVoice<sub>(currently not supported)</sub>**

{: .mt-8}
### Chat transcript request
{: .mb-4}
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
### Requests timeout <sub>since 4.3.0</sub>
{: .mb-4}
Outgoing requests to the bot AI are configured with a request timeout value. The request timeout defines the connection and response timeouts. i.e: what is the maximum time it should take to establish a connection and deliver a response.
> By default, each configured to 30 sec.   

When a request reaches its timeout, an error indication will be triggered and delivered to the SDKs handlers and to the hosting App.

As of android SDK version 4.3.0, the hosting app can configure the timeout value. The timeout can be increased or decreased as needed, according to a common response time observed on the connection to the server.   

The configuration is done using the `ConversationSettings`, provided by the app on `ChatController` creation.
 
```kotlin
val conversationSettings = ConversationSettings()
                           .requestTimeout(TIMEOUT_IN_MS)
```


{: .mt-8}   
### Features availability configurations
{: .mb-4}
Enable/Disable/Inspect support of some of the SDKs provided features.
- Autocomplete - Enable/Disable autocomplete suggestions. Effects only chat types that support this feature.
i.e: This configuration will take precedence if conflicts with current enabling state.
```kotlin
val conversationSettings = ConversationSettings()
                           .enableAutocompleteSupport(ENABLE)
```

- Feedback - Control support for AI chats.
Available with the following:   
`disableFeedback`, disables the feedback feature no matter what was configured on the Console.
`feedbackTimeout`, in case of [Timed feedback]({{'/docs/advanced-topics/feedback/timed-feedback' | relative_url}}), sets the chat idle timeout before it gets displayed. 
```kotlin
val conversationSettings = ConversationSettings()
                           .disableFeedback(DISABLE)
                           .feedbackTimeout(TIMEOUT)
```

- Typing indication - configure display for live chats

- Chatbar top header - configure display for live chats

- End chat over chat header - configure display

- Chat scroller display - scroll to bottom floating button

- Datestamp header - configure display and format


