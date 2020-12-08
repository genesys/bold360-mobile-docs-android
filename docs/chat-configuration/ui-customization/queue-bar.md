---
layout: default
title: Queue Bar
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/queue-bar
nav_order: 7
---

# Queue Bar {{site.data.vars.need-review}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
On live chat creation, if all available agents are occupied to their capacity, the chat will be added to a waiting queue.
The chat will be promoted in the queue, until position 0, indicating that the chat is no longer in the queue and is waiting for an agent acceptance.
{: .overview}

![]({{'/assets/images/queuebar.png' | relative_url}})
{: .image-40}


While in [queue state]({{'/docs/chat-configuration/tracking-events/chat-lifecycle#inqueue' | relative_url}}), the user can choose to cancel the chat.   
Chat canceling is handled as unavailable, and the user will be presented with unavailability form or message, according to the chat window configurations. 

![]({{'/assets/images/console-unavailability.png' | relative_url}})   
{: .image-70}

---

## Admin configurations
The following should be enabled, in order to have chats queue support, and to be notified of chat position in queue.

1. Automatic distribution  
   {: .light-title}
   ![]({{'/assets/images/ADC-console.png' | relative_url}})
   {: .image-70}

2. Show queue position
  {: .light-title}
  Check this option in order to be notified of user position in queue.

---

## Customize Queue Component
Queue position UI display is provided by the SDK, and can be customized by configuration settings or by an overriding custom queue component implementation.

{: .mt-8}
### Customizing by Configuration
Override `ChatUIProvider.queueCmpUIProvider.configure` method, in order to apply UI customizations to the provided SDKs UI component.   
By overridding the `configure` method you can set the component background, define the text style, the location of the bar on the screen, etc.

```kotlin
ChatUIProvider(context).apply {
    queueCmpUIProvider.configure = { adapter ->
            adapter.apply{
                setMargin(0, 10, 0, 0);
                setBackground(ContextCompat.getDrawable(context, 
                                    android.R.drawable.bottom_bar));
                setTextStyle(StyleConfig(16, 
                                        Color.RED, 
                                        getTypeface(context, "great_vibes.otf"));
                setPadding(10, 10, 10, 10);
                positionInChat = PositionInChat.Bottom
                ...
            }
        }
}
```

{: .mt-8}
### Custom queue component
Use your own custom component instead of the SDKs provided one.   
Create your custom `QueueFactory` implementation and apply it to the ui provider `overrideFactory` property:

```kotlin
queueCmpUIProvider.overrideFactory = object : QueueCmpUIProvider.QueueFactory {
        override fun create(context: Context): QueueCmpAdapter {
            return MyQueueImp(context)
        }
    }
```

##### Update position display
{: .strong-sub-title}

In order to be notified of queue position changes on your custom queue component, your component implementation should provide a `Notifiable` implementation which declares `Notifications.QueuePosition` notification as one of it's notifications.

> Once a UI component is created and added to the chat, if it provides an implementation of `Notifiable`, by `getNotifier` method, the implementation is automatically being registered to its listed notifications.

```kotlin
class MyQueueImp(context: Context) : QueueCmpAdapter {
    ...
    override fun getNotifier(): Notifiable? {
        return object : Notifiable {
            override fun onNotify(notification: Notification, dispatcher: DispatchContinuation) {
                if (notification.notification == Notifications.QueuePosition) {
                    (notification.data as? InQueueEvent?).let { queueEvent -> 
                        // get the queue position
                        queueEvent.data.position.toString()
                        // get the `Cancel chat` text, if available
                        queueEvent.cancelWaitBranded
                    }
                }
            }
            // declare a list of notifications this view should be registered to:
            override fun notifications(): ArrayList<String> {
                return arrayListOf(Notifications.QueuePosition)
            }
        }
    }
    ...
}
```

##### Cancel chat 
{: .strong-sub-title}

In order to activate chat cancel (end) from within your custom component do the following:
1. Implement the `setListener` method   
Save the provided function implementation, to be used for event passing from your UI component.
```kotlin
override fun setListener(listener: ((CmpEvent) -> Unit)?) {
    this.onCmpEvent = listener // save listener for later use
}
```
2. When the user chooses to cancel the chat, invoke the saved listener with a CmpEvent, identified with the queue UI component type `ComponentType.QueueStatusCmp` and with `CmpEvent.Activated` state, as follows:

```kotlin
this.onCmpEvent?.invoke(
                CmpEvent(ComponentType.QueueStatusCmp, CmpEvent.Activated))
```

---

## Cancel chat view 
The Queue component allows the user to cancel the chat while in queue state, using the `Cancel chat` view.   

{: .mt-6}
Enable/Disable display
{: .strong-sub-title}   

`Cancel chat` view display, depends on `ChatUIProvider.queueCmpUIProvider.queueUIConfig.enableCancel` configuration, which defaults to `true`.   
Do the following in order to change that configuration:
```kotlin
ChatUIProvider(context).apply {
    // `Cancel chat` view won't be available:
    queueCmpUIProvider.queueUIConfig.enableCancel = false
}
```

{: .mt-6}
Configure cancel message
{: .strong-sub-title}   

`Cancel chat` displayed text, is customizable via the chat window customizations. You can change it's default value and add more branding languages values by setting the text for `api#chat#unavailable_email` branding key.   
> The displayed text is branded according to the chat selected language.

---

## Listening to queue position changes
Queue position changes can be listened to by a [custom UI component](#update-position-display), as described before.    
In order to listened to position changes not from a custom UI implementation, you need to subscribe to the `Notifications.QueuePosition` notification using the `ChatController`.

```kotlin
chatController.subscribeNotifications(NotifiableImpl, 
                                      Notifications.QueuePosition, 
                                      ...[other notifications]...);
```
