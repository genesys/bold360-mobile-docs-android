# Version 3.5.2

In this version:

## Bold live chat related

- Support "High value + push chat" behavior:  
  Articles that need immediate escalation without asking have the value "High value + push chat". If an article with this value is presented do automatically the escalation.

## Bot ai chat related

- Added reporting for `Voice` and `Autocomplete` user interactions.

### SDK imports

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.2"
implementation "com.bold360ai-sdk.conversation:engine:3.5.2"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.2"
implementation "com.bold360ai-sdk.conversation:ui:3.5.2"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.60"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 3.5.1
In this version:

## Bold live chat related
- **Postchat form, SDKs implementation**
 
- **Fixes:**
  - Upload feature became disabled when upload icon configured as hidden.
    > Important: Upload image visibility configuration chage

## Bot ai chat related
- Channels icons as configured in bold360ai console
- Hint text for input field as configured in bold360ai console

## Breaking Changes
- StatementScope.isLive is no longer a function, but a property.
- ErrorCodes relocated to package "com.integration.core.annotations"


##
### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.1"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"
implementation "com.bold360ai-sdk.conversation:engine:3.5.1"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.1"
implementation "com.bold360ai-sdk.conversation:ui:3.5.1"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.60"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```


---


# Version 3.5.0
In this version:

- **SDK supports API 16+**
 ```diff
 - Known issue:
  Ticket form is not loaded properly on devices with API < 19
```
- **Fixes:**
  - While on TalkBack mode, only items with action are described with "double tap to activate.." instruction.
  ```diff
  -Known issue:
  While in `TalkBack` mode internal links are not being activated properly.
  ```
  - Improved Carousel items height calculation 

  - Improved parssing of span tag with style attribute

##
### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.0"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"
implementation "com.bold360ai-sdk.conversation:engine:3.5.0"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.0"
implementation "com.bold360ai-sdk.conversation:ui:3.5.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.50"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```


---


# Version 3.4.0
In this version:

## General
- **Autocomplete support** -    
New input field component was introduced. This field supports autocomplete, which is currently available only on Bot chats.
  ```diff
  - Breaking Changes: 
    Input field configurations are now available via `ChatInputUIProvider`
  ```
- **AccountInfo improvements** -    
   ```diff
   - Breaking Changes on Account class hierarchy.
     Account.info is now of type `SessionInfo`. (was ByteArray)   
     Account details such as `chatId`, `visitorId`, `providerConfig`, etc are available via this object.
  ```
- **Improving chat restore support**

- **_`Deprecated` method `onAccountUpdate`_ in ChatEventListener**   
  Account updates are received by AccountInfoProvider implementation.


## Bot related
- **_`Deprecated` - Conversation class_**   
BotAccount uses `SessionInfo` instead (id, holds the last conversationId)
-	**_`Deprecated` method `updateAccountInfo`_ in `AccountInfoProvider`**   
Use `update` method instead.
-	**_`Deprecated` property `lastConversation`_ in BotAccount**  
Use `info` member instead
- **Persistent options, incoming element UI customization**   
`PersistentOptionsUIProvider` supports customization of wrapping bubble element.
- **Fixes** - 
  - fixes related to feedback and readmore articles
  - fix for ticket typed channel activation
  - some UI fixes


## Bold related
- **_`Deprecated` - VisitorInfo class_**   
BoldAccount uses `SessionInfo` instead (id, holds the visitorId)
- **Department availability and Departments list**, requests support.   
Via `ChatAvailability` 
- **ChatId exposure** -    
chatId is available once a live chat is created and account is updated, via `BoldAccount.info` member.
- **Bot chat transcript to live agent** -    
Once a Bot chat is escalated to a live Bold chat, the bot chat content is passed to the live agent.
- **Live chatbar customization** 
  ```diff 
  - Breaking Changes:
    - was ChatBarComponent.ChatBarViewProvider now ChatbarCmpAdapter
    - Configuration for End component is handled by `configEndCmp` (was `updateUI)
    - Configuration for agent details component is handled by `configAgentCmp` (was `updateUI)
  ```

- **Fixes** - 
  - double event raising on upload press

##
### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.4.0"
implementation "com.bold360ai-sdk.core:accessibility:3.4.0"
implementation "com.bold360ai-sdk.conversation:engine:3.4.0"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.4.0"
implementation "com.bold360ai-sdk.conversation:ui:3.4.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.31"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```


---

# Version 3.3.2

In this version:

## Bot Chat related
- Fixed an issue with ticket channel activation.
- Reopen `customize` method in `ChatUIProvider`.

### Bold Chat related
- Fixed an issue that caused the agent’s details to change in the chatbar as the customer was typing.


#### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.3.1"
implementation "com.bold360ai-sdk.core:accessibility:3.3.1"
implementation "com.bold360ai-sdk.conversation:engine:3.3.2"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.3.2"
implementation "com.bold360ai-sdk.conversation:ui:3.3.2"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.31"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

## Version 3.3.1

In this version:

### Bot Chat related
- Fix for url channel activation.
- Fix for empty bubble content.

```diff
- Known issue: 

Ticket typed channels not working
```


#### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.3.1"
implementation "com.bold360ai-sdk.conversation:engine:3.3.1"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.3.1"
implementation "com.bold360ai-sdk.conversation:ui:3.3.1"
implementation "com.bold360ai-sdk.core:accessibility:3.3.1"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.31"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---


## Version 3.3.0

In this version:

### Bot Chat related
- Welcome message customization support by integrating app.
- Autocomplete standalone component

```diff
! Breaking Changes: 

# BotAccount location: import com.nanorep.nanoengine.bot.BotAccount
```
### Bold Live Chat related:
- Live chatbar: Agent name and avatar display
- Chat availability check support.
- Live chat language customization by integrating app

### General:
- Chat element changes listening support    
(`import com.nanorep.convesationui.structure.history.ChatElementListener`)

```diff
! Breaking Changes:

# HistoryProvider was deprecated and should not be used. Use full implementation of ChatElementListener instead.

# ChatUIProvider.customize method became internal and is not accesible for now.
  Dynamic configurations will be supported in the future.
```


#### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.3.0"
implementation "com.bold360ai-sdk.conversation:engine:3.3.0"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.3.0"
implementation "com.bold360ai-sdk.conversation:ui:3.3.0"
implementation "com.bold360ai-sdk.core:accessibility:3.3.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.31"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---


## Version: 3.2.4

In this version:

### Bot Chat related:

- On load article (welcome message)
- Custom user id - overriding default generated id
- Handover support -Third party chat support


### Bold Live Chat related:

- Passing user typing indication to agent. agent can now see when the visitor is typing.
- Chat bar display while live chat is in progress. Enables end live chat.
- File upload, supports working with SDK default upload mechanism and integrating with self implementation.


### General:

- Improve memory usage and images display on different device resolutions.
- Improvements in connections establishing and requests posting.
- Bugs fixes.

#### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.2.4"
implementation "com.bold360ai-sdk.conversation:engine:3.2.4"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.2.4"
implementation "com.bold360ai-sdk.conversation:ui:3.2.4"
implementation "com.bold360ai-sdk.core:accessibility:3.2.4"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.31"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

### Version: 3.2.3

**In this version:**

1. Bold Chat - Support "Skip prechat" and extra data configurations provided by app side.|
   https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_android_adv_present_forms.html
   
2. Bold Chat - Support queue position indication display Queue position UI is both configurable and overridable.
   https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_android_adv_chat_queue_position.html

3. LifeCycle events - There were added 2 new state events.
   - InQueue - raised when chat enters the queue
   - Pending - raised when chat was assigned to an agent but not yet accepted.

4. Base design implementation for "readmore" indication over the bubbles. 
   Support configurations change of the readmore indication.
   - Full article screen redesign, channels redesign.

#### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.2.3"
implementation "com.bold360ai-sdk.conversation:engine:3.2.3"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.2.3"
implementation "com.bold360ai-sdk.conversation:ui:3.2.3"
implementation "com.bold360ai-sdk.core:accessibility:3.2.2"
```

---

### Version 3.2.2
Release date March 13, 2019
   
**In this version:**
1. SDK default UI configurations and customisations upgrade.   

2. Typing indication when bold live agent starts typing.   

3. File upload enabling support. You can now integrate your existing file upload mechanism with 
the Bold360ai SDK.   

4. Upgrade to Bot V2 API.   

5. Bugs fixes - among them:
   - big images in responses, handling, prevents crashes and reduces memory usage. 
   - Bot timed feedback not stopping when live chat starts
   - outgoing message status indication updates to "ok" only if was passed successfully to an agent.
   - Adding chat disconnected message when a chat with live agent was disconnected, and can't be reconnected.


#### SDK imports
```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.2.2"
implementation "com.bold360ai-sdk.conversation:engine:3.2.2"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.2.2"
implementation "com.bold360ai-sdk.conversation:ui:3.2.2"
implementation "com.bold360ai-sdk.core:accessibility:3.2.2"
```

---

### Version 3.2.1

Release date: February 21, 2019

In this version:

1. we have some improvements - requests dispatching mechanism was upgraded and improved.

2. Bugs fixes: app crashes, carousel sizing, some regressions fixes.

3. We've upgraded kotlin coroutines version which is now `1.1.1`

4. We're now using the new gradle import methods,`api` and `implementation`

```gradle
implementation "com.nanorep.core:sdkcore:3.2.1"
implementation "com.nanorep.conversation:engine:3.2.1"
implementation "com.nanorep.conversation:chatintegration:3.2.1"
implementation "com.nanorep.conversation:ui:3.2.1"
implementation "com.nanorep.core:accessibility:3.2.1"
```
---


### Version 3.2.0

Release date: January 31, 2019

This release contains the following Bold360 Android SDK Features:

* Bold live chat with agent:
  - Identify customers and continuation of chats
  - Chat forms display. SDK provides default implementation for the preChat form. SDK enables forms override
    and display by app side. 
  - Localization

* lifecycle state events and notifications subscription.

* Bot chat support:
  - Feedback & Escalation on bot responses.
  - Responses types - Carousel, Options, Channels, videos, etc.

>**Notice.** Current limitations   
Imports are needed to all of listed below:
```gradle
implementation "com.nanorep.conversation:ui:3.2.0"

implementation "com.nanorep.conversation:chatintegration:3.2.0"
implementation "com.nanorep.conversation:engine:3.2.0"
implementation "com.nanorep.core:sdkcore:3.2.0"
implementation "com.nanorep.core:accessibility:3.2.0"
implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.20"
implementation "org.jetbrains.anko:anko-commons:0.10.8"

implementation "com.android.support:appcompat-v7:28.0.0"
implementation "com.android.support:design:28.0.0"
implementation "com.android.support.constraint:constraint-layout:1.1.3"
implementation "com.makeramen:roundedimageview:2.3.0"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:0.27.0-eap13"
implementation "com.google.code.gson:gson:2.8.2"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

 