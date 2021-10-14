---
layout: default
title: Incoming options
np_toc: true
nav_exclude: true
---

# Incoming message options {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Persistent options
Incoming bot response can have persistent options. Those options will not dissappear after user selection.   

### How to customize
This kind of incoming element can be customized by overriding default implementation of the PersistentOptionsUIProvider.
- The options style can be configured by `PersistentOptionsUIProvider.optionsStyleConfig`   
- The wrapping bubble can be customized by `PersistentOptionsUIProvider.contentStyleConfig`
  ```kotlin
  ChatUIProvider(context).apply {
      
      chatElementsUIProvider.incomingUIProvider.persistentOptionsUIProvider.apply {
          // customize options text style:
          optionsStyleConfig = StyleConfig(...)
          
          // customize the wrapping bubble look:
          contentStyleConfig = { adapter -> 
              adapter.textBackground(...)
              
              // do more adjustments
          }
      }    
  }
  ```

- Options look can also be customized by override as follows:
```kotlin
ChatUIProvider(context).apply {
    
    chatElementsUIProvider.incomingUIProvider.persistentOptionsUIProvider.overrideFactory = 
                object : UIInfoFactory {
                    override fun info(): ViewInfo {
                        /* return your own ViewInfo object - 
                                which defines the layout resource for the options */
                    }
                }
}
```

## QuickOptions
Incoming bot response can have several options to the user to choose from. Those options are not constant and will disappear after user action.

### How to customize
1. Customization by override.   
    {: .strong-sub-title}   

    Apply your own layout resource. 
    ```kotlin
    ChatUIProvider(context).apply {
        
        chatElementsUIProvider.incomingUIProvider.quickOptionsUIProvider.overrideFactory = 
            object : QuickOptionUIProvider.QuickOptionsFactory {
                override fun info(): ViewInfo {
                    /* return your own ViewInfo object - 
                                    which defines the layout resource for the options */
                }
            }
    ```
2. Customization by properties change on `QuickOptionsUIProvider`.   
   {: .strong-sub-title} 

    ```kotlin
    uiProvider.chatElementsUIProvider
        .incomingUIProvider.quickOptionsUIProvider.apply { 
                optionsMargin = xxx // dp value
                startMargin = xxx // dp value
        }
    ```

## Channels
Channels usage allow you to set up the escalation paths and rules on your Bold360 ai chats. Use this ability to forward visitor engagements to another escalation channel such as chat, email, or click-to-call.

- Channels are a sub type of QuickOptions. Channels are used for user escalation actions.   
- Channels may appear as response options or on article page.  
- Channels can be created on the [Bold360ai console](https://support.nanorep.com/1009689492).

### Customizing channels icons
{: .mb-4}

- Setting icon to channel on Bold360ai console:
{: .strong-sub-title}

![]({{'/assets/images/console-channeling-icons.png' | relative_url}})
{: .image-70}

- Overriding SDK default icons. 
  {: .strong-sub-title}
  This can be done using the following ways:

  - Overriding the channels drawable resources:
    * Phone : `R.drawable.call_channel`
    - Chat : `R.drawable.chat_channel`
    - Ticket: `R.drawable.email_channel`

  - Overriding the SDKs icons generating method:
    {: .mt-5}

    ```kotlin
    ChatUIProvider(context).apply {
      chatElementsUIProvider.incomingUIProvider.quickOptionsUIProvider.overrideFactory =
          object : QuickOptionUIProvider.QuickOptionsFactory {
              override fun generateDefaultChannelImage(context: Context, channelType: Int): Drawable? {
                  // return drawable according to channelType:
                  return when (channelType) {
                      Channels.PhoneNumber -> ... // return null to prevent default icon display
                      Channels.Chat -> ...
                      Channels.Ticket -> ...
                      else -> null
                  }
              }
          }
    }
    ```

📚 How to prevent default SDK icon display
{: .strong-sub-title .mt-6}

```kotlin
ChatUIProvider(context).apply {
    this.chatElementsUIProvider.incomingUIProvider.quickOptionsUIProvider.overrideFactory = object: QuickOptionUIProvider.QuickOptionsFactory {
        override fun generateDefaultChannelImage(context: Context, channelType: Int): Drawable? {
            return null 
        }
    }
}
```

## Ticket channel
{: .mt-8}
Channel of type ticket may be configured with one of the following actions:   

Show a `Contact form`
{: .strong-sub-title .mt-3 .mb-4}

![]({{'/assets/images/contact-form.png' | relative_url}})
{: .image-40 .mb-6}

Open a Url   
{: .strong-sub-title .mt-4}
- Regular URLs are passed to the hosting app over `ChatEventListener:onUrlLinkSelected` for further handling
- Linked article URLs (of the format: `nanoreplinkid://56342653`) are handled by the SDK. The article which has the configured id will be injected into the chat.

![]({{'/assets/images/custom-url.png' | relative_url}})
{: .image-70 .mb-6 .mt-3}

Run a custom script
{: .strong-sub-title .mt-4}
❗ Not supported by the SDK.
{: .mb-4}
![]({{'/assets/images/ticket-channel-script.png' | relative_url}})
{: .image-70 .mb-6}
