---
layout: default
title: System Message
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/system-message
nav_order: 5
---

# System Message {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview 
...

## Constomization
Currently system messages look can be changed only by `override`.

- ### Customize by override layout resouce   
  
  Create your own layout resources with the following names:   
  - Regular messages: system_message_regular (inner textview should keep SDK known id: message_textview)
  - Removable messages: system_message_removable (inner textview should keep SDK known id: message_textview)

- ### Customize by override SystemUIProvider factory
  {: .mt-5}
    1. _Implement SystemFactory:_ 
        - override `removableSystemInfo` method, for changing the layout for the removable system messages (appears for a specific state and than are removed from the chat)
        - override `info` method for changing the regular messages display.

        
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