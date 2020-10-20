---
layout: default
title: Standalone
parent: Autocomplete
grand_parent: Advanced Topics
nav_order: 1
permalink: /docs/advanced-topics/autocomplete/standalone
---

# Autocomplete on standalone component
{: .no_toc }
need work
{: .btn .btn-orange }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
The SDK provides a standalone bot autocomplete component, `BotAutocompleteFragment`.
This component supports self state restoring, like on rotation mode changes.
Landscape mode is not supported for autocompletion suggestions display. The suggestions to the text changes that were done in `Lanscape mode` will be displayed once device is back on `Portrait mode`. 
{: .overview }

## Setup
### Create autocomplete parameters
`BotAutocompleteFragment` uses `BotCompletionViewModel` to get its parameters.
`BotCompletionViewModel` should be obtained from `ViewModelProvider`, and customed as needed.   
### Obtaining `BotCompletionViewModel`
  When obtaining the `BotCompletionViewModel`, use the containing activity as the source for the ViewModelProvider.   
 
```kotlin
  // when fragment's parameters are filled by a fragment:
  val botViewModel = ViewModelProviders.of(activity!!).
                                get(BotCompletionViewModel::class.java)

  // when fragment's parameters are filled by the activity:
  val botViewModel = ViewModelProviders.of(this).
                                get(BotCompletionViewModel::class.java)                                
```
### Setting `BotCompletionViewModel` values

```kotlin

val botViewModel = ...

val account = BotAccount(...)
botViewModel.botChat.account = account 
// or set the botViewModel.botChat, if there's already an active instance 
// botViewModel.botChat = myBotChatInstance
```

### Add to the application
 The `BotAutocompleteFragment` can  be added to an activity or to another fragment as with a regular fragment.
```kotlin
fragmentManager.beginTransaction()
            .add(R.id.root_layout, BotAutocompleteFragment()).commit()
```

### Optional - Listening to events
Set observers on the events you would like to be notified of.

```kotlin
// obtaining the BotCompletionViewModel object
val botViewModel =  ...

// set observer for errors:
botViewModel.onError.observe(this, Observer{ error ->
        error?.run { 
            //do something
        }
    })

// set observer for article selection from the suggestions:

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

// set observer for conversationId changes:
botViewModel.onConversationIdUpdate.observe(this, Observer { id ->
        // do something
    })
    
```

### Optional - Customize appearance 
The component UI configuration defined by `AutocompleteViewUIConfig`.
In order to change the config default properties values, create your own object and pass it on the `BotCompletionViewModel` (passed to the standalone).
```kotlin
val botViewModel = ViewModelProviders.of(...

val customedUIConfig = ChatAutocompleteUIConfig(context).apply { 
    // change properties values
}

botViewModel.uiConfig = customedUIConfig
```

**Tip**
In order to preseve the conversation created on state restoring of the containing activity/fragment, do not override the BotAccount on the `BotCompletionViewModel` object if already exists.

```kotlin
val botViewModel = ViewModelProviders.of(activity!!).get(BotCompletionViewModel::class.java);
        
if (!botViewModel.botChat.hasSession) {
    botViewModel.botChat.account = BotAccount(...)
}
```