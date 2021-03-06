# Version 4.1.1

### Features
- #### Validated live chat support 
  The SDK now provides the way to pass a secured encrypted data string, using the BoldAccount, for creating a secured validated live chat with agent.

- #### Addition of chat form related resources
  In order to enable more flexibility of overriding the chat forms look, we've added the following:
  - The ability to override the padding size and the margin between, of the chat form fields. `R.dimen.form_field_padding` and `R.dimen.form_fields_gap`
  - A separate resource color for the rating views background, `R.color.form_rating_field_background`
  - A style resource for the form fields hint appearance, `@style/FormHintTextAppearance`

### Fixes 
- Disabling the chat input field, once all active chats were ended. User can't continue typing messages.
- Fix for chat form fields background override by resource.
- Fix for form fields hint color override by resource.



---

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.1.1"
implementation "com.bold360ai-sdk.conversation:engine:4.1.1"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.1.1"
implementation "com.bold360ai-sdk.conversation:ui:4.1.1"
implementation "com.bold360ai-sdk.core:accessibility:4.1.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```
---


# Version 4.1.0

### Features
- #### Chat elements
  - **Chat elements are uniquely identified by a String typed Id property**, instead of their timestamp. timestamp of chat elements is no longer unique.
  - StorableChatElement was updated accordingly and the method `getId()` was added.
  - ChatElementListener: Addition of onUpdate and onRemove methods.
  - All chat types are supports the Id property usage for identifying and adding messages to the chat. 
  - Chat elements structure was changed, so serialization and deserialization of elements was updated.    
  Backward support of old elements deserialization was integrated in order to prevent current stored chats from being lost. (As long as the `storageKey` will be provided on the storage fetched elements)
  - In case of previous stored chats, a migration tool is provided on this version, to convert old scheme elements to the new ones.   
    > Follow [migrating your chat](./How-to-migrate-to-4.1.0.md) for more details.
 
  ```diff
  - Breaking changes and Deprecations:
  ```
  - ##### _ChatElement_
    - relocated to package: `com.nanorep.convesationui.structure.elements`**

  - ##### _StorableChatElement_
    - Interface is now located on package: **`com.nanorep.convesationui.structure.elements`**
    - getId() method was added. Returns a unique String identification of the element.
    - The deprecated method `getStorableContent():String` was removed

  - ##### _ChatElementListener_
    <u>Deprecated methods:</u>
    - `onRemove(timestampId: Long)` 
    - `onUpdate(timestampId: Long, item: StorableChatElement)`    

    <u>Replacement methods:</u>
    - `onRemove(id: String)` 
    - `onUpdate(id: String, item: StorableChatElement)`
    
  - ##### _AgentType_
    - Enum was deprecated.
    - Deprecated `agentType` property was removed from chat element classes.
    
  - ##### _ClearBoldChatSession.Builder_
    - Constructor doesn't receive a context as parameter. The context should be provided on `build` method.

- #### Input field
  Scrolling support addition enables content of more than 6 lines.
##

### Fixes

- Connectivity receiver leak errors
- Fix of the crash that happened if malformed Bold API key was provided. Now it fails with an error.
- Fix of the crash that was experienced when rotating the device while a chat form was presented.
- Fix of crash when changing the device language, mid chat. 
- Fix of carousel readout crash.
- Chat forms: Replacing hard-coded color and dimension values with resources, to enable override and night mode configured replacements by the hosting App
- Fix of the issue that if the pre-chat form was canceled, due to activity finish state, the cancellation callback was not triggered, and the chat was not canceled properly.
- Fixed the issue that if multiple messages were sent in a fast time frame some messages were not visible in the chat, although they were stored in history and sent properly to the agent.



---

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.1.0"
implementation "com.bold360ai-sdk.conversation:engine:4.1.0"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.1.0"
implementation "com.bold360ai-sdk.conversation:ui:4.1.0"
implementation "com.bold360ai-sdk.core:accessibility:4.1.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```
---


# Version 4.0.4

### Fixes
- Numerical strings with length longer than 3 digits are no longer being obfuscated by SDK.

---

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.0.3"
implementation "com.bold360ai-sdk.conversation:engine:4.0.4"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.0.1"
implementation "com.bold360ai-sdk.conversation:ui:4.0.3"
implementation "com.bold360ai-sdk.core:accessibility:4.0.1"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 4.0.3

### Features
- Hands-free experience is now added to the voice-to-voice mode. When the option is turned on, the microphone is automatically enabled once the answer read out is done.

### Fixes
- Fixed an issue that caused reading out persistent options twice in voice-to-voice mode.
- When customizing the chat look and feel, multiple `SendUIConfig` instances were reachable from the `ChatUIProvider`. We simplified it to have a single one that is reachable under `ChatUIProvider.chatInputUIProvider.sendCmpUIProvider.uiConfig`.   

  > #### Usage of ???ChatAutocompleteUIConfig.sendUIConfig??? was deprecated.
  
### ChatController API
- `ChatLoadedListener` can be provided also after ChatController creation, for following chat start/restore operations.

### Known issue
- Ticket typed channel is not supported on devices with API level lower than 19  

---

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.0.3"
implementation "com.bold360ai-sdk.conversation:engine:4.0.3"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.0.1"
implementation "com.bold360ai-sdk.conversation:ui:4.0.3"
implementation "com.bold360ai-sdk.core:accessibility:4.0.1"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 4.0.2

Fixed on this version:

- **Voice to voice:** response readout is activated on voice recorded requests only.

- **Full article display**: Displayed content and title matches opened article, also on postback responses.

- Prevent images and video images from being cut, on wide devices resolutions.

```diff
- Deprecations:
```
> - InputViewListener - typingStarted, typingEnded were replaced with inputStarted, inputEnded.
> - ChatInputData - onSendInput was replaced with onSend


```diff
- Known Issue:
```
> - Duplicate configuration options are available for Send component, but only one is currently effective, and should be used.   
  **`ChatUIProvider.chatInputUIProvider.uiConfig.sendUIConfig`**
___


```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.0.2"
implementation "com.bold360ai-sdk.conversation:engine:4.0.2"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.0.1"
implementation "com.bold360ai-sdk.conversation:ui:4.0.2"
implementation "com.bold360ai-sdk.core:accessibility:4.0.1"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 4.0.1

In this version:

### Bot related
- **UserId management** - The SDK generates a userId via BE API, on Bot chat creation,   if one was not provided by the embedding App.   
  In order to identify chats sessions as belong to the same user in the reporting, the same userId should be used. Newly generated userId is available to the embedding App, via `AccountInfoProvider.update` implementation. 
```diff
- Breaking Changes
```
> - The Embedding App is responsible to **save the userId**, once created, and **provide it on `BotAccount.userId`** for successive chats creation of the same account. 

- **Multi answer design** - Bot responses which contains multiple answers of kind `inlineChoice`, are displayed as persistent options. Meaning the options are not disappears when one is selected.

### Fixes
- Improve SDK allocated resources release.
- Fix active links on bot responses, while escalated live chat is in progress.

___


```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.0.1"
implementation "com.bold360ai-sdk.conversation:engine:4.0.1"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.0.1"
implementation "com.bold360ai-sdk.conversation:ui:4.0.1"
implementation "com.bold360ai-sdk.core:accessibility:4.0.1"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 4.0.0

In this version:

### Voice to voice
- An extension feature to the speech recognition, Text to speech, responses to recorded requests can be read to the user.
- Configurable TTS engine
- Embedding app can alternate the text before it is read to the user.
- Voice support level is configurable on ConversationSettings

### Messaging chat
> If you are interested of the Messaging capabilities of the Mobile SDK please reach out to your Customer Success Manager

### TLSv1.2 protocol support
- SDK supports TLSv1.2 secured connections on lower API level devices (< 21)

##

#### Improvements
- ChatController
  - ChatController can now be used to create multiple chats, no need to re-instantiate.
  - Chats end is generated by one of the chatting parties: user, live-agent, or by the embedding app. Chats are no longer being closed automatically.
  - New APIs and properties:</u>
    - `onChatInterruption` - notify the SDK when something was activated on the device that may interrupt the regular chat flow, like incoming/outgoing calls.   
    - `destruct` - The ChatController instance will clear all its resources and active chats.
    - `wasDestructed` - Indicates if ChatController was destructed and can no longer be used.
    - `terminateChat` - Ends **all** current open chats.
    - `endChat` - Ends only active open chat.
    - `startChat` - start a new chat with account, with the same ChatController instance.   
    - `restoreChat` - Continue open chat even if the chat UI was removed. Also can be used when the chat fragment is restored by the app, and to start new chats.
    - `HandoverHandler` - Handover handler can be set to the ChatController instance at any time.

#### Fixes
- Bot - articles parsing.
- Bot - Welcome message request doesn't increases the engagements and interactions counters.

#### Changes and Deprecations

```diff
- Breaking Changes   
```
- <u>Handover</u> - Chat elements related events are not passed automatically to the `ChatElementListener` implementations. Best practice: extend the abstract HandoverHandler class and use its base class injection methods.
- SDK doesn't ends chats automatically anymore. Chat can be ended by user, live agent or the embedding App. ChatController destruction doesn't ends the chats, only releases their resources. [see ChatController new APIs](#improvements)
- ErrorCodes definition was relocated to package "com.integration.core.annotations"
- `DrawablePosition` was removed, since it was a duplicate of `CompoundDrawableLocation`

```diff
- Deprecations
```
> All deprecations replacements and comments are available via javaDoc/KDOC (Android studio quick help)
- Configuring voice support on `ConversationSettings` and `ChatInpuData` replaced with VoiceSettings
- SessionInfo.update - overrides only properties with the same key (use override method for a complete replacement)
- Live session properties, such as, ChatId, Department, applicationId, replaced with the identical properties located on `com.integration.core.LiveSession` 
- Elements injection methods on ChatDelegate were deprecated. 
- `ChatInputData.textInputHint` replaced with `ChatInputData.inputHints`
- Constructor deprecation on ContentChatElement class hierarchy.
- `VisitorDataKeys` were deprecated and replaced with `SessionInfoKeys`
- `ChatbarCmpConfig` - _drawableLocation_????and??_compoundDrawablesPadding_ were deprecated and replaced with??_ChatbarCmpConfig.drawableConfig_,??which include them both.

```diff
+ Known issues
```
- Bot - Articles with iframe tag for **embedded videos**, should not contain empty properties, such as `allowfullscreen`. Every property should have a value set to it.

___


```gradle
implementation "com.bold360ai-sdk.core:sdkcore:4.0.0"
implementation "com.bold360ai-sdk.conversation:engine:4.0.0"
implementation "com.bold360ai-sdk.conversation:chatintegration:4.0.0"
implementation "com.bold360ai-sdk.conversation:ui:4.0.0"
implementation "com.bold360ai-sdk.core:accessibility:4.0.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.72"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---


# Version 3.5.6

In this version:

## Article full view Fixes
- Fix of the issue that the Article title was not presented in the full screen mode display of articles
- Fix of the issue that articles were truncated for the end users. If an html encoded character was present in an article, the text after the character was not presented.

### SDK imports

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.5"
implementation "com.bold360ai-sdk.conversation:engine:3.5.6"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.5"
implementation "com.bold360ai-sdk.conversation:ui:3.5.6"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.61"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---


# Version 3.5.5

In this version:

## General Fixes
- Fix of the issue that the placeholder, in the text input, changed when a chat was channeled to a live agent.


## Bold live chat Fixes

- Initial questions submitted through the pre-chat form, are visible to the end user, as the first, end user, sent message.
- Postchat form submission, returns with no errors.


### SDK imports

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.5"
implementation "com.bold360ai-sdk.conversation:engine:3.5.5"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.5"
implementation "com.bold360ai-sdk.conversation:ui:3.5.5"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.61"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 3.5.4

In this version:

## General Chat related
- Fix of the issue that HTML encoded special characters (like &nbsp;) are not displayed at all.
- User messages are sent as typed - Fix of the the issue that the <br/> tag was visible for the live agent when the end user added a line break in a message.
- Fix of the issue that if you had additional visual customization on system messages the message had double border


## Bot ai chat related

- Re-enabling the feedback gathering method that is presented with a small delay after showing the bot answer as a separate question from the bot.
- When presenting "High value + push chat" value messages, first the answer is presented than than automatic channeling is performed.
- Fix of the issue that article links did not open the linked article


### SDK imports

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.4"
implementation "com.bold360ai-sdk.conversation:engine:3.5.4"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.3"
implementation "com.bold360ai-sdk.conversation:ui:3.5.4"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.61"
implementation "com.google.code.gson:gson:2.8.6"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 3.5.3

In this version:

## General Chat related

- Expose an interface to programmatically inject a user query in a conversational bot.
>Note: This feature was already available, docs have been updated

## Bot ai chat related

- Initialize Entities support

### SDK imports

```gradle
implementation "com.bold360ai-sdk.core:sdkcore:3.5.2"
implementation "com.bold360ai-sdk.conversation:engine:3.5.3"
implementation "com.bold360ai-sdk.conversation:chatintegration:3.5.2"
implementation "com.bold360ai-sdk.conversation:ui:3.5.2"
implementation "com.bold360ai-sdk.core:accessibility:3.5.0"

implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.60"
implementation "com.google.code.gson:gson:2.8.5"
implementation "android.arch.lifecycle:extensions:1.1.1"
```

---

# Version 3.5.2

In this version:

## Bold live chat related

- Support "High value + push chat" behavior:  
  Articles that are configured with "High value + push chat" option, are immediatelly escalate to the first `Chat` channel when recieved.

## Bot ai chat related

- Added analytics reports for `Voice` and `Autocomplete` user's interactions.

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
- Fixed an issue that caused the agent???s details to change in the chatbar as the customer was typing.


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

 