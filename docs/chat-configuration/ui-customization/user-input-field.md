---
layout: default
title: User Input Field
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/user-input-field
nav_order: 2
---

# User Input Field {{site.data.vars.force-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
Usually appears at the bottom of the chat screen. Contains the users typed/recorded message, until it was sent.   
The behavior and functionality of the input field is defined by the active chat type.   
{: .overview}

> e.g., The **upload icon** will appear only on live chats and only if was enabled by configuration.

## Available features
- Typing and sending messages 
- [Autocomplete](/docs/advanced-topics/autocomplete/in-chat)
- [Voice recording and readout control](/docs/advanced-topics/voice)
- [File upload](/docs/advanced-topics/file-upload) on live chats

### Configure features
Some of the available features, can be configured by the hosting App. Using `ConversationSettings` object, that can be provided on ChatController creation, you can configure the following:

- [`voiceSettings(VoiceSettings)`](./docs/chat-configuration/chat-settings#voice-control)

- `enableAutocompleteSupport(Boolean)`

---

## send  button
...

### Send UI override
In order to override the send view set `ChatUIProvider.chatInputUIProvider.sendCmpUIProvider.overrideFactory` with your view factory.
> Custom implementation must implement `InputControlersHandler`


---

## Hint configuration

- Hint configuration for AI chats is done on the [bold360ai console]({{'/assets/images/ai-hint-config.png' | relative_url}}).
  > Escalated chats will not effect the AI configured hint.

- Hint configuration for Live chats is done on the [bold admin console]({{'/assets/images/live-hint-config.png' | relative_url}}). 

- Override hint text string resource `R.string.type_message_here`.   
  This resource will be used, if none of the above was provided, to the current active chat.

---

## UI customization
The user input field can be customized by:

- ### Configuration   
  The SDK provides a configurable _user input field_ implementation.
  The default configuration can be changed via `ChatUIProvider.chatInputUIProvider.uiConfig` and `ChatUIProvider.chatInputUIProvider.SendCmpUIProvider.uiConfig` <sub>(from 4.1.0)</sub>
  
  ```kotlin
  val customProvider = ChatUIProvider(context).apply{
      chatInputUIProvider.uiConfig.apply{
        uploadImage = ...
        belowPopupBackground = ... // autocomplete suggestions open downward image
        abovePopupBackground = ... // autocomplete suggestions open upward image
        noPopupBackground = ... // input field background when there are no autocomplete suggestions 
        suggestionUIConfig = ... // defines config for the autocomplete suggestions rows 
        inputStyleConfig = ... // the style of the input field text
        ...
      }

      chatInputUIProvider.SendCmpUIProvider.uiConfig.apply{
          speecherUIConfig.apply{
            speakerImage = ... // speaker icon, presented while response is being read to the user
            micImage = ... // recording voice icon
          }
          
          sendImage = ... // message send icon
      }
  }

  val chatController = ChatController.Builder(context)
    ...
    .chatUIProvider(customProvider)
    .build(...)
  ```

  |![]({{'/assets/images/input_field_1.png' | relative_url}}){: .image-70}|![]({{'/assets/images/input_field_2.png' | relative_url}}){: .image-70}|![]({{'/assets/images/input_field_2.png' | relative_url}}){: .image-70}
  |---|---|---|
  |![]({{'/assets/images/input_field_3.png' | relative_url}}){: .image-70}|![]({{'/assets/images/input_field_4.png' | relative_url}}){: .image-70}|![]({{'/assets/images/input_field_5.png' | relative_url}}){: .image-70}
{: .mb-8}


- ### Override
  In case a custom view is needed, instead of provided SDKs implementation, set `ChatUIProvider.chatInputUIProvider.overrideFactory` with your view factory.

  > Custom implementation must implement `ChatInputViewProvider`
  
  