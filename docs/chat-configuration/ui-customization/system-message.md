---
layout: default
title: System Message
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/system-message
nav_order: 5
---

# System Messages {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview 
System originated messages, used for indicating chat state,notify of errors, providing the user with instructions, etc.   
There are fixed messages and removable messages.
### Fixed messages
Appears in the chat and will stay there, Those messages are also passed on the `ChatElementListener` for history preservation.

### Removable messages
Messages that are displyed in the chat for a period of time, until a fulfillment of some condition is met, usually during specific chat state), than automatically are removed. Those messages are not passed to the `ChatElementListener` and will not appear when chat gets restored from history.      
> e.g., The `Wait` message that appears to the user while he waits for his live chat to accepted by an agent. 

## Injecting System messages
The hosting app can inject system messages after the chat was created, using the `ChatController.post` API.
```kotlin
chatController.post(SystemStatement(message))
```

## Constomization
Currently system messages display, can only be customized by [`override`]({{'/docs/chat-configuration/ui-customization/how-it-works#override' | relative_url}}).

- ### Customize by overriding layout resouce   
  
  Create your own layout resources with the following names:   
  - Regular messages: system_message_regular (inner textview should keep SDK known id: message_textview)
  - Removable messages: system_message_removable (inner textview should keep SDK known id: message_textview)

- ### Customize by overriding SystemUIProvider factory
  {: .mt-5}
    1. _Implement SystemFactory:_ 
        - Override the `removableSystemInfo` method, to change the removable system messages layout.
        - Override the `info` method to change the regular messages display.

        
    2. _Set `SystemUIProvider.overrideFactory` with your implementation._    

        ```kotlin
        class MySystemMessageFactory: SystemFactory{
            override fun info():ViewInfo{
                return ....
            }
            override fun removableSystemInfo():ViewInfo{
                return ....
            }
        }
        
        val chatUI = ChatUIProvider(context).apply {
            this.chatElementsUIProvider.systemUIProvider.overrideFactory = mySystemMessageFactory
        }

        ChatController.Builder(context).apply{
                //... do some initiations
                chatUIProvider(chatUI)
            }
        ```