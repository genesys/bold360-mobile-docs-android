---
layout: default
title: Chat Bar
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/chat-bar
nav_order: 6
---

# Chat Bar {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
A UI component, which appears over the chat screen, while a live chat is in progress. 
The Chatbar displays the following:
 1. Active agent details - name and avatar.
 2. `End Chat` clickable view to end current live chat.
{: .overview}

![]({{'/assets/images/chatbar.png' | relative_url}})
{: .image-40}

## How it works
The UI component data is being updated on chat acceptance and on every operator change indication. 

> Currently operator change events are not passed to the hosting app.
   In order to do so, you can override the SDKs provided `Chatbar`, and pass update events from your implementation.

## UI customization
- The UI component look can be configured by overriding the [`configure`]({{'/docs/chat-configuration/ui-customization/how-it-works#configure' | relative_url}}) method of `ChatBarCmpUiProvider`.   
See `ChatbarCmpAdapter` for available configurations.
    ```kotlin
    ChatUIProvider(this).apply {
        chatBarCmpUiProvider.configure = { adapter ->
            adapter.configAgentCmp(ChatbarCmpConfig().apply { 
                this.textStyle = StyleConfig(...)
            })
            adapter.configEndCmp(ChatbarCmpConfig().apply {...})
            adapter.setBackground(...)
            adapter
        }
    }
    ```

- **Overriding agent avatar placeholder image** - can be done, by overriding the image resource on the hosting App resources (resource id - `R.drawable.agent`)

- **Remove "end chat" view display** from the chatbar - can be done using `ConversationSettings`.
    ```kotlin
    conversationSettings.disableEndLiveChatSupport()
    ```


## Custom Chatbar override
The provided `Chatbar` implementation can be replaced with your custom implementation of `ChatbarComponent.ChatbarViewProvider`, by overriding [`overrideFactory`]({{'/docs/chat-configuration/ui-customization/how-it-works#override' | relative_url}}) property of the `ChatBarCmpUIProvider`. 
```kotlin
ChatUIProvider(this).apply {
    chatBarCmpUiProvider.overrideFactory = 
        object : ChatBarCmpUIProvider.ChatBarFactory {
                override fun create(context: Context)
                             : ChatbarComponent.ChatbarViewProvider {
                    // return your ChatbarComponent.ChatbarViewProvider implementation
                }
        }
}
```


