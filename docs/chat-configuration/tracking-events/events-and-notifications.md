---
layout: default
title: Events and Notifications
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 3
permalink: /docs/chat-configuration/tracking-events/events-and-notifications
---

# Events and Notifications {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Chat events and notifications

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
Among those events: [lifeCycle events](ttps://github.com/bold360ai/GlobalDocs/wiki/ChatLifecycleAndroid), links/urls/phone-number selections, etc.

### Listening to url navigation
  The SDK receives to url clicks from articles/quick options and channels.
  Implement `ChatEventListener.onUrlLinkSelected(url: String)` method in order to receive those click events.

  > In order to configure url typed channels (in-app navigation), follow this [guide](https://github.com/bold360ai/GlobalDocs/wiki/Bold360ai-Console-Configurations#configure-in-app-navigation-channel).



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