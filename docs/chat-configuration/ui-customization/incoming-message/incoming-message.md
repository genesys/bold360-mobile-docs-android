---
layout: default
title: Incoming message
parent: UI Customization
grand_parent: Chat Configuration 
permalink: /docs/chat-configuration/ui-customization/incoming-message/
has_children: true
nav_order: 2
has_toc: false
---

# Incoming message {{site.data.vars.need-work}}

Table of content:
{: .text-delta}
1. [How to customize](/docs/chat-configuration/ui-customization/incoming-message/#how-to-customize)
2. [Readmore](/docs/chat-configuration/ui-customization/incoming-message/#readmore)
3. [Message options](/docs/chat-configuration/ui-customization/incoming-message/incoming-options)
4. [Feedback](/docs/advanced-topics/feedback)
5. [Carousel](/docs/chat-configuration/ui-customization/incoming-message/carousel)


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

