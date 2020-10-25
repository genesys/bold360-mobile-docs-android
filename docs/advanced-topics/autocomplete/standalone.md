---
layout: default
title: Standalone
parent: Autocomplete
grand_parent: Advanced Topics
nav_order: 2
# permalink: /docs/advanced-topics/autocomplete/standalone
---

# Standalone autocomplete component
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The SDK provides a standalone AI autocomplete component, called `BotAutocompleteFragment`.   
This UI component can be located in your fragment, activity, etc, and will automatically connect to a bot AI source, for autocomplete suggestions fetching.
{: .overview }

The standalone component support self state restoring on device rotation mode changes.   
Suggestions will not be displayed while the device is on `Landscape mode`. Text changes will be registaered and the relevant suggestions will be displayed once the device is back on `Portrait mode`. 

---

## Setup

### Create autocomplete parameters
`BotAutocompleteFragment` uses `BotCompletionViewModel` as its parameters source.
`BotCompletionViewModel` should be obtained from `ViewModelProvider`, and configured with the autocomplete parameters as needed.

- Obtain `BotCompletionViewModel` {: .strong-sub-title}   
  When obtaining the `BotCompletionViewModel`, use the containing activity as the source for the ViewModelProvider.   
 
  ```kotlin
  val botViewModel = ViewModelProviders.of(activity).
                                  get(BotCompletionViewModel::class.java)
  ```

- Apply autocomplete parameters values {: .strong-sub-title}   
  Once you have the `BotCompletionViewModel` instance, set its properties as needed, for the autocomplete functionality and display.

    - `botChat`<sub>Mandatory</sub> - If you already have an active bot chat, just set this    parameter to point to your reference, otherwise, set the provided instance with a [`BotAccount`](/docs/chat-configuration/chat-account/bot-account).
    
      ```kotlin
      val botViewModel = ... // Obtain...
      botViewModel.botChat.account = BotAccount(...) 
      // or set the botViewModel.botChat, if there's already an active instance 
      // botViewModel.botChat = myBotChatInstance
      ```
    - `uiConfig`<sub>Optional</sub> - Set UI properties to customize the view look as needed.
    - `openingQuery`<sub>Optional</sub> - If configured, will automatically be inserted to the input field.
    - `onError`, `onSelection` and `onConversationIdUpdate`<sub>Optional</sub> - [Can be observed](#listening-to-events) in order to be updated with the component events. 


### Component display    
{: mt-10}   
 The `BotAutocompleteFragment` can  be added to an activity or to another fragment.
```kotlin
fragmentManager.beginTransaction()
            .add(R.id.root_layout, BotAutocompleteFragment()).commit()
```

### Listening to events
{: mt-10}   
Optional, set observers to the events you would like to be notified of.

```kotlin
val botViewModel =  ... // obtain 
// observe errors:
botViewModel.onError.observe(this, Observer{ error ->
        error?.run { 
            //do something
        }
    })
// observe suggestions selection:
botViewModel.onSelection.observe(this, Observer { selection ->
        // a utility method "getArticle" is passed over Selection object
        // BotAutocompleteFragment provides a "getArticle" method as well that can be used.
        selection?.getArticle?.invoke(selection.articleId) { result ->
            result.error?.run { 
                // handle error 
            } ?: result.data?.run { 
                // handle article
            }
        }
    })
// observe conversationId changes:
botViewModel.onConversationIdUpdate.observe(this, Observer { id ->
        // do something
    })
```

### UI configurations   
{: mt-10} 
The component UI configurations are defined by `AutocompleteViewUIConfig`.
In order to change the default UI look, create instance of `AutocompleteViewUIConfig`, configure its properties as needed and set it to `BotCompletionViewModel.uiConfig` propety.
```kotlin
val botViewModel = ViewModelProviders.of(activity)...
val customedUIConfig = ChatAutocompleteUIConfig(context).apply { 
    // change properties values
}
botViewModel.uiConfig = customedUIConfig
```

---

> **Tip**: In order to preseve the chat, on state restoring, of the containing activity/fragment, do not override the BotAccount on the `BotCompletionViewModel` object if already exists.
```kotlin
val botViewModel = ViewModelProviders.of(activity!!).get(BotCompletionViewModel::class.java);       
if (!botViewModel.botChat.hasSession) {
    botViewModel.botChat.account = BotAccount(...)
}
```
{: mb-10}