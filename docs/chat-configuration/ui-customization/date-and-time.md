---
layout: default
title: Date and Time
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/date-and-time
nav_order: 9
---

# Date and Time {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
In the chat there are 2 main time related elements:

- **The dates notifications in the chat** - grouping elements of the same date messages 

![](/assets/chat_datestamp.png)
{: .image-40}

- **The messages timestamp** - usually appears arround the message bubble

![](/assets/chat_timestamp.png)
{: .image-40}

---

## Datestamp display
In order to change default datestamp display:
1. Implement `DatesampFormatFactory`
2. Pass that implementation to the ConversationSettings using `ConversationSettings.datestamp` method.

```kotlin
class MyDatestampFactory : DatesampFormatFactory {
    override fun formatDate(datestamp:Long):String{
        return ...
    }
}

val settings = ConversationSttongs()
                .datestamp(enable, myDatestampFactory)
                //... set more settings

ChatController.Builder(context).apply{
        //... do some initiations
        conversationSettings(setting)
    }
```
In the SDK we provide 2 predefined factories: 
- `SimpleDatestampFormatFactory` using the pattern `"EEE, d MMM, yyyy"` to display dates
- `FriendlyDatestampFormatFactory` (which is the default) - provides more common display, using `today`, `yesterday` phrases.

---

## Timestamp display
There are 2 ways of setting a new look to the timestamp display.

1. Configure by [chat settings](/docs/chat-configuration/chat-settings)
    {: .strong-sub-title}
  
    Using `ConversationSettings.timestampConfig` method.
    ```kotlin
    val settings = ConversationSttongs()
            .timestampConfig(enable, TimestampStyle(
                time_pattern, text_size, text_color, font_typeface
            ))
            //... set more settings

    ChatController.Builder(context).apply{
    //... do some initiations
    conversationSettings(setting)
    }
    ```

2. Overriding [configure/customize](./how-it-works)
    {: .strong-sub-title}

    Overriding the configure and/or customize methods and set the desired look for the timestamp on `ChatUIProvider.chatElementsUIProvider.[incomingUIProvider](./incoming-message)` and/or `ChatUIProvider.chatElementsUIProvider.[outgoingUIProvider](./outgoing-message)`.   
      
    ```kotlin
    val chatUI = ChatUIProvider(context).apply {
            this.chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
                    this.setTimestampStyle(TimestampStyle(time_pattern, text_size, text_color))
                }
                adapter
            }

            this.chatElementsUIProvider.outgoingUIProvider.configure = { adapter ->
                    this.setTimestampStyle(TimestampStyle("hh:mm aa", 10, Color.parseColor("#aeaeae")))
                }
                adapter
            }
        }

    ChatController.Builder(context).apply{
            //... do some initiations
            chatUIProvider(chatUI)
        }
    ```