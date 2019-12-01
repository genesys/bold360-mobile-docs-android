# Version 3.5.5

> **New**
- Chat bar attachment (Agent information & End chat button).
- Event tracker attachment.
- Search view place holder attachment.
- Minor file upload bug fixes.

# Version 3.5.4

> **New**
- Passing account data when escalating from botChat to liveAgent.
- Predefined pre chat form with extra data.
- Minor bug fixes.

# Version 3.5.2

This release contains the following bold360ai iOS SDK Features/ Fixes:

```diff
! Breaking Changes

Passing `LiveAccount` data is done using `account.extraData` instead of `account.info`
```

> **New**
- Support iOS 13.
- Standalone autocomplete feature. 

> **Bot Chat related**
- Autocomplete is supported.

> **Live Chat related:**
- Bot conversation transcript is sent to live console.

# Version 3.4.8

This release contains the following bold360ai iOS SDK Features/ Fixes:

```diff
! Breaking Changes

`HistoryProvider` was deprecated and should not be used. 
Use full implementation of `ChatElementDelegate` instead.
```
> **Bot Chat related**
- Welcome message customization support by integrating app.

> **Live Chat related:**
- Chat availability check support.

# Version 3.4.7

This release contains the following bold360ai iOS SDK Features/ Fixes:

* Support File Upload - Live Chat.

# Version 3.4.0

Release date: April 28, 2019

This release contains the following bold360ai iOS SDK Features/ Fixes:
 
* Support History
* Support restore chat

# Version 3.3.9

Release date: April 17, 2019

This release contains the following bold360ai iOS SDK Features/ Fixes:
 
* Support all chat view controller attachments (modal, navigation, child vc...).
* Framework stabilization.

# Version 3.3.2

Release date: April 14, 2019

This release contains the following bold360ai iOS SDK Features/ Fixes:

* Icon Positioning

# Version 3.3.1

Release date: March 31, 2019

This release contains the following bold360ai iOS SDK Features/ Fixes:

* Support skip `prechat` including extra params.
* Support queue position component on live chat with agent.

>Note: This version contains **breaking changes**

* Class name `AccountParams` was replaced by `BotAccount`.

# Version 3.3.0

Release date: March 14, 2019

This release contains the following bold360ai iOS SDK Features/ Fixes:

* File upload API.
* Fixed article view presentation bug.
* Native typing indication by design.
* New default bot/ agent icons.

# Version 3.2.1

Release date: February 14, 2019

This release contains the following bold360ai iOS SDK Features/ Fixes:

* Error Handling Support.
* Typing indication Support.
* Minor search view input fixes.

# Version 3.2.0

Release date: January 30, 2019

This release contains the following bold360ai iOS SDK Features:

* Lifecycle State Events.
* Chat forms display. 
     - SDK enables forms override and display by app side.
     - SDK provides default implementation only for the `preChat` form.
* Chat UI Customizations.
* Start Directly with Live Agent (No Bot first).

# Version 3.0.0

Release date: December 5, 2018

This release contains the following Bold360 iOS SDK Features:

* Request and submit answers to pre-chat
* Identify customers
* Send and receive chat messages
* Bot Support
* Feedback & Escalation 
