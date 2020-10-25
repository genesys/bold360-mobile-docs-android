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

> Exp: The **upload icon** will appear only on live chats and only if was enabled by configuration.

## Available features
- Typing and sending messages 
- [Autocomplete](/docs/advanced-topics/autocomplete/in-chat)
- [Voice recording and readout control](/docs/advanced-topics/voice)
- [File upload](/docs/advanced-topics/file-upload) on live chats

## send  button

## Configuring Hint
  - Configure hint text on [bold360ai console.](/assets/user-input-hint-config.png)
    > Currently this configuration will apply only to chats that were started with **ai**.

  - Configure hint text by overrding SDKs string resource: `R.string.type_message_here`.


## Configure features
Some of the available features, can be configured by the hosting App. Using `ConversationSettings` object, that can be provided on ChatController creation, you can configure the following:

- [`voiceSettings(VoiceSettings)`](./docs/chat-configuration/chat-settings#voice-control)

- `enableAutocompleteSupport(Boolean)`

## UI configurations
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
```

|![](/assets/input_field_1.png)|![](/assets/input_field_2.png)|![](/assets/input_field_2.png)
|---|---|---|
|![](/assets/input_field_3.png)|![](/assets/input_field_4.png)|![](/assets/input_field_5.png)



- ### Override
In case a customed view is needed instead of supplied SDKs implementation, set `ChatUIProvider.chatInputUIProvider.overrideFactory` with your view factory.
> Custom implementation must implement `ChatInputViewProvider`
  
- In order to override the send view set `ChatUIProvider.chatInputUIProvider.sendCmpUIProvider.overrideFactory` with your view factory.
> Custom implementation must implement `InputControlersHandler`
