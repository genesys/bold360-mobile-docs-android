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
...

## Listening to chat elements changes
In order to be able to be notified of all elements changes in the chat, implement `ChatElementListener`, and register as follows:
```kotlin
ChatController.Builder(context)....
    .chatElementListener(listenerImpl)

//OR

chatController.setChatElementListener(listenerImpl)
```
### Possible updates 
`onReceive`(req) - on element inserted to the chat.   
`onRemove`(opt) - element was removed from the chat.   
`onUpdate`(opt) - element data was updated.

`onFetch`(opt) - implement this in order to enable display of previous chat content. (Historic content)


### Ongoing events
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
                if(results != null) {
                    Log.i(My_TAG, "Got notified for form results for form: " +
                            results.getData() +
                            (results.getError() != null ? (", with error: " + results.getError()):""));
                } else {
                   Log.w(My_TAG, "Got notified for form results but results are null");
                }
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