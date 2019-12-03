> This article will explain how to enable queue component and configure/ override it.

# Bold chat Queue
## Info
When user starts a bold chat session, until an agent is assigned to the user's chat and accept it, the chat can be added to a queue.
 
When previous chats will end the user's chat will be promoted in the queue until position 0, means the chat is no longer in the queue and the chat is waiting for agent acceptance.

> User can end the chat while in the queue with [life cycle state](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_android_adv_chat_lifecycle.html) (`StateEvent.InQueue`) or while waiting for acceptance (`StateEvent.Pending`). The user will be presented with unavailability form if enabled otherwise a message.

### How to enable support
1. Enable `[automatic distribution]` in bold console.   
Some configurations can be changed, like queue limit, agent concurrent chat limit etc.
2. Enable `[Show queue position]` in bold console, in order to be notified of user position in queue.

### Queue Component
Queue position UI display is provided by the SDK. 
>The component can be configured (text size font etc) or be override if programmer wants his own implementation.
>`QueueCmpAdapter` interface should be implemented by the overriding component.    

#### How to Configure
provide `configure` method for `ChatUIProvider.queueCmpUIProvider`
```kotlin
chatUIProvider.queueCmpUIProvider.configure = { adapter -> 
    adapter.setBackground(drawable)
    adapter.setTextStyle(StyleConfig(12, context.resources.getColor(R.color.colorTextLight)))
...
}
```
#### How to override
Set a new `QueueFactory` implementation for the overrideFactory member.
```kotlin
chatUIProvider.queueCmpUIProvider.overrideFactory = MyQueueFactory()

class LoggerQueueFactory : QueueFactory{
    
    val tag = "QueueCmpLogger"

    override fun create(context: Context): QueueCmpAdapter {
        return object :QueueCmpAdapter{
            override fun setTextStyle(styleConfig: StyleConfig) {
                Log.i(tag, "got styleConfig: $styleConfig")
            }

            override fun setBackground(background: Drawable?) {
                 Log.i(tag, "got background drawable: $background")
            }

            override fun setLocation(left: Int, top: Int, right: Int, bottom: Int) {
                 Log.i(tag, "got cmp position: [$left,$right,$")
            }

            override fun locateOnTop() {
                  Log.i(tag, "got position cmp at top")
            }

            override fun enable(enable: Boolean) {
                  Log.i(tag, "got enable = $enable")
            }

            override fun getNotifier(): Notifiable? {
                  return object : Notifiable {
                       override fun onNotify(notification: Notification, dispatcher: DispatchContinuation) {
                           if (notification.notification == Notifications.QueuePosition) {
                               val data = notification.data as? Map<String, Any>
                               data?.run {
                                   Log.i(tag, "got position update: position=${data["position"]}, enableCancel=${data["enableCancel"]}")
                               }
                            }
                       }

                       override fun notifications(): ArrayList<String> {
                           return arrayListOf(Notifications.QueuePosition)
                       }
                    }
              }

              override fun clear() {
                  Log.i(tag, "activating clear")
              }

              override fun pause() {
                  Log.i(tag, "activating pause")
              }

          }
     }
}

```

