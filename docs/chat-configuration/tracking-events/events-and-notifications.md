---
layout: default
title: Events and Notifications
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 3
---

# Events and Notifications
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
While the chat is in progress, the SDK triggers and notifies about changes, events and actions.   
The hosting app can listen and register to those events and notifications. 
The hosting app, by catching those events, can be updated with the chat state and can react, and handle actions that are not provided by the SDK.

## Chat elements events
While the chat is in progress `chat elements` are added, updated, removed and fetched on the chat.
`Chat elements` are the items that you see on your chat, each item has a purpose and type.   
Every action that applies to a `chat element` may trigger an event. i.e. addition of incoming message to the chat, will trigger an `onReceive` event.

### How to listen
In order to listen to chat elements events, implement `ChatElementListener`, and apply it to the `ChatController` as follows:
```kotlin
// on ChatController creation:
ChatController.Builder(context)....
    .chatElementListener(listenerImpl)
//OR
chatController.setChatElementListener(listenerImpl)
```
### ChatElementListener events: 
- `intercept`<sub>(optional)</sub> - Called when a chat element is about to be injected to the chat.   
- `onReceive`<sub>(required)</sub> - Called when a chat element was injected to the chat.   
- `onRemove`<sub>(optional)</sub> - Called when a chat element was removed from the chat.   
- `onUpdate`<sub>(optional)</sub> - Called when a chat element data was updated.
- `onFetch`<sub>(optional)</sub> - Called when a chat starts, fetching previous chat elements from history implementation, if one available.

> All methods except for `intercept` are related to chat history maintainig, and can be viewed on [History Support]({{'/docs/advanced-topics/history' | relative_url}}) page.

### Intercepting chat elements <sub>[since 4.5.0]({{'/docs/release-notes/#version-450' | relative_url}})</sub>
{: .mt-7 .mb-3}

The SDK opens a new listening channel to hosting apps, were they can be notified of chat elements **before** injection to the chat, and be able to reject elements and effect the chat flow.   
In contarst to the `onReceive` method, the `intercept` is called on all chat elements; timed messages, options, feedback, etc. `intercept` can also be used by the hosting app to react, activate, announce to chat elements. e.g. `intercept` can be used to announce with accessibility API of specific chat elements.

> Exceptional:     
  Chat headers, datestamp elements are not interceptable. In order to controll their display check [Date and Time]({{'/docs/chat-configuration/ui-customization/date-and-time/#disabling-datestamp-display' | relative_url}}) page.

`intercept` 
{: .strong-sub-title .mt-6}
- Chat elements are passed to this method under the `ElementModel` interface hierarchy.
- Return value is boolean, defines if the element should be processed and added to the chat, or be rejected. When a chat element is being rejected, its following logic is rejected as well. e.g. rejected outgoing message, will not be sent to the bot/agent. 
- Synced method and should not handle long tasks.
- Default implementation doesn't intercept elements, all are accepted and injected to the chat.

Potential usages
{: .strong-sub-title .mt-6}
- [Accessibility]({{'/docs/faq/accessibility' | relative_url}}) service API usage for announcing of elements before they are added to the chat.
- Intercept informative elements as, system messages, from being injected to the chat, and display the needed information on apps external UI component.
- Disable features for all or specific messages. e.g. prevent [instant feedback]({{'/docs/advanced-topics/feedback/instant-feedback' | relative_url}}) on bot reponses.
- Prevent bad language on the chat, by intercepting those messages.


> Disclaimer:   
{: .strong-sub-title .mt-6}
    `intercep` implementation is optional, therefore, when the default behavior is being overriden   
    by hosting app implementation, the responsibility of chat integrity falls on the app. 


👉 [See interception sample](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/topics/ElementsInterceptionChat.kt)
{: .mt-6}

---

## Ongoing events
During chat progress, the SDK raises other events such [lifecycle state events]({{'/docs/chat-configuration/tracking-events/chat-lifecycle' | relative_url}}) and action requests, when user selects url links, phone numbers or file upload on live chat.   
In order to listen to those events, implement `ChatEventListener` and set it to `ChatController`, as follows:

```kotlin
// on instance creation:
ChatController.Builder(context)...
    .chatEventListener(ChatEventListenerImpl)
// OR later
chatController.chatEventListener = ChatEventListenerImpl 
```

---

## Subscribing to notifications
ChatController provides a notifications service, for tracking events, such, file upload progress and status, agent typing and forms submission results.   

Subscribing the the notifications service is done as follows:
1. Implement Notifiable interface, to receive notifications.
  ```kotlin
  // Implementing the Notifiable interface:
  class NotificationsReceiver : Notifiable {
      override fun onNotify( notification : Notification, dispatcher: DispatchContinuation) {
          when (notification.notification) {
              ChatNotifications.PostChatFormSubmissionResults -> {}
              Notifications.UploadStart -> {}
              ...
          }
      }
  }
  ```

2. Select desired notifications from the supported notifications.
> <details>      <summary>Supported notifications:</summary>
    PostChatFormSubmissionResults, UnavailabilityFormSubmissionResults, OperatorTyping, Notifications.UploadEnd, Notifications.UploadStart, Notifications.UploadProgress, Notifications.UploadFailed, LanguageChanged, Notifications. QueuePosition      </details>

3. Subscribe to the notifications service with `ChatController` API.   
   ❗ Since we save the subscribing `Notifaiable` implementation as `WeakReference` on the SDK, make sure to keep hard reference to your instance on the app side, to avoid instance lost. (e.g. class member)
    
   ```kotlin
   val notificationsReceiver = NotificationsReceiver() 
   chatController.subscribeNotifications(notificationsReceiver, ChatNotifications.PostChatFormSubmissionResults, Notifications.UploadStart, ...)
   ```

⚜️ Once receiving notifications is no longer needed, or on resources release, **unsubscribe** from the notifications service with `ChatController` API, as follows:
{: .mt-8}
```kotlin
chatController.unsubscribeNotifications(notificationsReceiver);
```