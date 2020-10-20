---
layout: default
title: In chat
parent: Autocomplete
grand_parent: Advanced Topics
nav_order: 1
permalink: /docs/advanced-topics/autocomplete/in-chat
---

# Autocomplete in AI chat
{: .no_toc }
need work
{: .btn .btn-orange .fs-3 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
Chat SDK supports autocomplete for chats with AI.     
While user is typing a query to the bot, if feature is enabled, he will be presented with suggested queries, relevant to the typed content.   
{: .overview}

<img alt='uploads bar' src='images/Android/autocomplete_bot.png' width=40% style="margin:16px"/>

### How to enable/disable chat autocomplete support

### Enable/Disable on client side settings</U>   
  Set the feature enable status on the `ConversationSettings` that can be provided to `ChatController.Builder`.
  
```kotlin
  val settings = ConversationSettings().apply {
      // ...
      enableAutocompleteSupport(enable) // enable = true/false
  }

  val chatController = ChatController.Builder(context)  
                        .conversationSettings(settings)                                                   
                        .build(account, ...) 
                        
```
## 
 **!! Notice:** _Client side settings overrides console settings._   
    But, in case the autocomplete was enabled on the client side but disabled on the bold ai console, on the account settings, though the autocomplete is enabled and passes requests, no suggestions will be received nor displayed.   
##

### How to configure
Autocomplete configurations should be set by `ChatUIProvider.chatInputUIProvider.uiConfig`
```kotlin
val uiProvider = ChatUIProvider(context).apply { 
                    chatInputUIProvider.uiConfig.apply { 
                        // configure properties as u want
                    }
                }

ChatController.Builder(this).apply {
    chatUIProvider(uiProvider)
}
```

- #### How to configure the Autocomplete Hint
  - Configure hint text on [bold360ai console.](./images/Android/autocomplete-hint-config.png)
    > Currently this configuration will apply only to chats that were started with **ai**.

  - Configure hint text by overrding SDKs string resource: `R.string.type_message_here`.


### How to override
Autocomplete view can be replaced by user implementation.   
```kotlin
val uiProvider = ChatUIProvider(context).apply { 
                    chatInputUIProvider.overrideFactory = 
                        object : ChatInputUIProvider.ChatInputUIProviderFactory {
                                override fun create(context: Context): ChatInputViewProvider {
                                    return ... // return implementation here
                                }
                        }
                 }       
```
---

