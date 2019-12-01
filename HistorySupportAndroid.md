# Chat History service

The SDK provides the tools to enable, integrating Apps, to create chats history mechanism, and support chat continuation.   
In order to listen to chat changes for history support, implement fully `ChatElementListener` and register as follows:

```kotlin
// exp. for history support implementation
class ChatElementListeningImpl : ChatElementListener{
    override fun onReceive(item: StorableChatElement) {
        // store item to chat history
    }

    override fun onRemove(timestampId: Long) {
        // remove item from chat history
    }

    override fun onUpdate(timestampId: Long, item: StorableChatElement) {
        // update saved item in chat history
    }

    override fun onFetch(from: Int, direction: Int, callback: HistoryCallback?) {
        // fetch page of history items starting from "from"
    }
}


ChatController.Builder(context)....
    .chatElementListener(chatElementListeningImpl)

//OR

chatController.setChatElementListener(chatElementListeningImpl)
```

**The following operations are notified over listener**
- **onFetch** - Fetches previously stored conversation history.
- **onRemove** - Remove stored elements from history. (by timestampId)
- **onReceive** - handle chat elements that are added to the chat. (store to history/report, etc)
- **onUpdate** - Update elements that were stored in history. (by timestampId)


## What is stored in history?
Elements that should be stored in history implements the `StorableChatElement` interface.   
**_Notice_** - Storable element contains a `storageKey`, this key should NOT be changed nor deleted, otherwise
history will not be fetched properly.   
When history should be loaded the SDK passes a `fetch` request with the position history should be fetched from. The HistoryProvider implementation defines the amount of data that will be fetched.  
History data is identified by the `StorableChatElement.getTimestamp()` value




























