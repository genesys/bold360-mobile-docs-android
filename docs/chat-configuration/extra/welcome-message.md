---
layout: default
title: Welcome Message
nav_exclude: true
---

# Welcome Message
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
AI chats can be configured to start with a preconfigured article, this will be fetched on chat start, and be displayed as first chat message. 
{: .overview} 
The `Welcome Message` will appear only once on chat start. If the chat is a continuance chat and has messages history, the `Welcome Message` will not be reloaded.  

`Welcome Message` article can have images, persistent options, carousel, channels, etc.   
Quick options and channeling of the `Welcome Message` will be accessible until first user query/action.  

![]({{'/assets/images/welcome-and-faqs.png' | relative_url}})
{: .image-40}

---

## How to configure
1. Configure on bold360ai admin console

![]({{'/assets/images/welcome_console.png' | relative_url}})
{: .image-70}

Configured `Welcome Message` id is available on the `cnf` API response labled as `onloadQuestionId`.   
   
2. Configure on chat account
`Welcome Message`, article id, can be configured on the `BotAccount`.
```kotlin
BotAccount(...).apply{
    welcomeMessage = WELCOME_MESSAGE_ID
}
```

- `Welcome Message` configuration on BotAccount has presidency over console configuration.

- If welcome message id was set to an invalid/non-existing article id, no welcome message will be displayed, error will be passed to `ChatEventListener.onError`.   

---

## How to disable
In order to prevent the `Welcome Message` display, no matter if configured on the console, set the welcome message id to `BotAccount.None`.   
> _Setting welcome message id to `null`, will not be effective, as if was not configured on the account._

```kotlin
BotAccount(...).apply{
    welcomeMessage = BotAccount.None
}
```
 ---

## Related Topics
 - [`FAQs Message']({{'/docs/chat-configuration/extra/faqs-message' | relative_url}})