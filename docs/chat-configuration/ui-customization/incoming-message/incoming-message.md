---
layout: default
title: Incoming message
parent: UI Customization
grand_parent: Chat Configuration 
permalink: /docs/chat-configuration/ui-customization/incoming-message/
has_children: true
nav_order: 3
---

# Incoming message {{site.data.vars.need-work}}
{: .no_toc}

## Table of contents 
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Message options](./incoming-options)
- [Feedback]({{ '/docs/advanced-topics/feedback' | relative_url }})
- [Carousel](./carousel)

---

## Overview
The chat partner(chatbot, agent) side messages.   
Displays textual and HTML formatted messages.
The incoming message component also supports the display of avatar image and timestamp. 
{: .overview}

Chatbot messages may be constructed by multiple UI components, depends on the message content.
The incoming message component contains the textual content of the message, persistent options, readmore.
Other message properties such channels quick options and feedback, are displyed by separate components. 

---

## How to customize
The incoming message component supports all [customization methods]({{ '/docs/chat-configuration/ui-customization/how-it-works#customization-methods' | relative_url }})
Customization options are defined by the configuration adapter `ExtendedBubbleContentUIAdapter`. The component implementation implements this adapter and configured customization applied to it on component creation, and on data set.

Customizing SDKs implementation
{: .eg-class}
```kotlin
ChatUIProvider(context).apply {
    chatElementsUIProvider.incomingUIProvider.apply {
        // on creation customization
        configure = { adapter: BubbleContentUIAdapter ->
                        adapter.apply {
                            setTextStyle(StyleConfig(14, Color.RED, Typeface.SANS_SERIF))
                            setBackground(ColorDrawable(Color.GRAY))
                            setAvatar(ContextCompat.getDrawable(context, R.drawable.avatar))
                        }
                    }

        // Dynamic customization on data update
        customize = { adapter: BubbleContentUIAdapter, element: IncomingElementModel? ->
                        element?.takeIf { it.elemScope.isLive }?.let {
                                adapter.apply {
                                    setAvatar(ContextCompat.getDrawable(context, R.drawable.agent))
                                    setTextStyle(StyleConfig(10, Color.WHITE))
                                    setBackground(ColorDrawable(Color.RED))
                                }
                            }
                        adapter
                    }
    }
}
```            

Overriding default implementation
{: .eg-class}
```kotlin
ChatUIProvider(context).apply {
    chatElementsUIProvider.outgoingUIProvider.apply {
        overrideFactory = 
            object : IncomingElementUIProvider.IncomingBubbleFactory {

                // simple incoming message component - text, avatar, timestamp
                override fun createIncoming(context: Context): BubbleContentAdapter =
                                                                CustomIncomingView(context)

                // extended incoming message, 
                // supports chatbot special message additions - persistent options, readmore
                override fun createExtendedIncoming(context: Context): ExtendedBubbleContentAdapter =
                                                                        CustomExtendedIncomingView(context)
            }
    }
}
```


---

## Readmore indication <sub><sup>(configurable only)</sup></sub>
`readmore` component configuration options are defined by `ReadmoreAdapter`.   
In order to customize the `readmore` component display, access `readmoreUIProvider` from `ChatUIProvider` and override its [`configure`]({{ '/docs/chat-configuration/ui-customization/how-it-works#adapter-configure' | relative_url }}) method.

```kotlin
ChatUIProvider(context).apply {
    chatElementsUIProvider.incomingUIProvider
                            .readmoreUIProvider.configure = { adapter:ReadmoreAdapter -> 
                                    adapter.apply {
                                        setReadmoreStyle(StyleConfig(16,
                                                                adapter.uiContext.resources.getColor(R.color.colorTextDark),
                                                                Typeface.create("sans-serif-light", Typeface.NORMAL)))
                                        alignReadmore(UiConfigurations.Alignment.AlignStart)
                                    }
                                }
}
```
- `readmore` indication text, is a string resource (`R.string.read_more`), and can be override by the integrating app.


### Define message length display threshold
...

