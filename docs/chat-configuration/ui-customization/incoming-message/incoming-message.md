---
layout: default
title: Incoming message
parent: UI Customization
grand_parent: Chat Configuration 
permalink: /docs/chat-configuration/ui-customization/incoming-message
has_children: true
nav_order: 2
---

# Incoming message {{site.data.vars.need-work}}
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Message options](/docs/chat-configuration/ui-customization/incoming-message/incoming-options)
- [Feedback](/docs/advanced-topics/feedback)
- [Carousel](/docs/chat-configuration/ui-customization/incoming-message/carousel)


## How to customize
...
 
---

## Readmore indication <sub><sup>(configurable only)</sup></sub>
- `readmore` indication view is configurable via `ReadmoreAdapter`, as follows:   

```kotlin
ChatUIProvider(context).apply {
    
    chatElementsUIProvider.incomingUIProvider.readmoreUIProvider.configure = { 
        adapter:ReadmoreAdapter -> 
        
        adapter.apply {
            alignReadmore(...)
            setReadmoreStyle(...)
            ...
        }
    }        
}
```
- `readmore` indication text, is a string resource (`R.string.read_more`), and can be override by the integrating app.


### Define message length display threshold
...

