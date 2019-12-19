# Chat Restoring
In order to restore a chat, the integrating app should implement `ChatElementListener.onFetch`.
[<sub>Read more</sub>](./HistorySupportAndroid)
## Restore chat after activity destroy
  In case ChatController is being created by the activity, and is being destroyed with the activity. Once the activity is restored, the ChatController **should be recreated**.  
   
  **_<sub> In case the chat fragment is restored, with the activity, it should be removed from the activity, before the new ChatController creation.</sub>_**

## Restore chat with consistent ChatController
  In case the ChatController outlive the chat containing activity, as in the case of device rotation, while activity doesn't handle rotation changes. When activity gets restored, the ChatController can be applied with the newely created chat Fragment. Chat will be restarted with the provided account, unless `ChatElementListener.onFetch` is implemented.
  ```kotlin
  chatController.onChatFragmentRestored(restoredFragment, accountInfo)
  ```
   
  _In order to **restore** the chat, the integrating app should implement `ChatElementListener.onFetch`._

## Device rotation - Orientation changes
- In case the containing _activity is destroyed on rotation changes_, a chat restore can be done as mentioned above.
- In case the containing activity is configured to _handle rotation changes_, the activity and its fragments won't be destroyed, and therefore no further handling is needed.
