---
layout: default
title: Events and Notifications
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 3
# permalink: /docs/chat-configuration/tracking-events/events-and-notifications
---

# Events and Notifications {{site.data.vars.need-work}}
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

### Intercepting chat elements <sub>since 4.5.0</sub>
{: .mt-7}
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
- Accessibility service API usage for announcing of elements.(todo: link to sample)
- Intercept informative elements as, system messages, from being injected to the chat, and display the needed information on apps external UI component.
- Disable features for all or specific messages. e.g. prevent [instant feedback]({{'/docs/advanced-topics/feedback/instant-feedback' | relative_url}}) on bot reponses.


> Disclaimer:   
{: .strong-sub-title .mt-6}
    `intercep` implementation is optional, therefore, when the default behavior is being overriden   
    by hosting app implementation, the responsibility of chat integrity falls on the app. 

---

## Ongoing events
In order to be notified over events that occurs during chat lifecycle, implement `ChatEventListener` and pass it on `ChatController` creation, as follows:
```kotlin
ChatController.Builder(context)...
    .chatEventListener(ChatEventListenerImpl)
```
Among those events: [lifeCycle, state events]({{'/docs/chat-configuration/tracking-events/chat-lifecycle' | relative_url}}), links/urls/phone-number selections, etc.

### Listening to url navigation
  The SDK receives to url clicks from articles/quick options and channels.
  Implement `ChatEventListener.onUrlLinkSelected(url: String)` method in order to receive those click events.

  <details close markdown="block">
  <summary>Configuring in-app navigation channels on Bold360ai console.</summary>
  
    Follow those steps:   
    
    1. Navigate to Channeling -> Channeling Policy

    2. Create a new Channel.

    3. Fill up the next fields:
        * Channel name and description
        * Who’s the target audience
    
    4. At `What To Do` select `Ticket`
    
    5. Fill the relevant fields:
        * Label
        * Button action -> `Open Custom URL`
        * Fill the Link Url with the inApp navigation prefix (example: inApp://MainFragment)
    
    6. Click on Save Settings

</details>
  
---

## Subscribing to notifications
ChatController provides a notifications service. You can subscribe to this service by indicating on what notifications you want to be notified of.
```java
chatController.subscribeNotifications(NotifiableImpl, notification, notification,...)
```
**Currently we support the following notifications:**
- ChatNotifications.PostChatFormSubmissionResults
- ChatNotifications.UnavailabilityFormSubmissionResults
- TrackingEvent.OperatorTyping

In order to subscribe to notifications you need to implement the `Notifiable` interface, and pass it on the subscription request.
```java
// Implementing the Notifiable interface:
class NotificationsReceiver implements Notifiable {
    @Override
    public void onNotify(@NotNull Notification notification, @NotNull DispatchContinuation dispatcher) {
        switch (notification.getNotification()) {
            case ChatNotifications.PostChatFormSubmissionResults:
            case ChatNotifications.UnavailabilityFormSubmissionResults:
                FormResults results = (FormResults) notification.getData();
                // get the submitted formType:
                String formType = results.getFormType();
                
                // Check if we got error on submission:
                NRError error = results.getError(); 

                results.getData(FieldKey.Email)
                break;
        }
    }
}

// declaring notifications handler as a member: 
private Notifiable notificationsReceiver = new NotificationsReceiver();

// subscribing to notifications:
chatController.subscribeNotifications(notificationsReceiver, ChatNotifications.PostChatFormSubmissionResults,
                ChatNotifications.UnavailabilityFormSubmissionResults);
```
Once the notification are no longer needed or you want to release services, call to unsubscribe.
```java
chatController.unsubscribeNotifications(notificationsReceiver);
```