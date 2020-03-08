
# Handover
**Handing** the control of the chat to a provided custom `ChatHandler` implementation.  
The handover is activated by escalating from a Chat typed channel, which is configured with `custom provider`.

Handover `ChatHandler` implementation should be provided on `ChatController` creation.
```kotlin
val chatController = ChatController.Builder(context)
                                .chatHandoverHandler(myHandoverHandler)
                                ...
                                .build(account,...)
```

### How to implement
1. Extend `HandoverHandler`.   
2. Override ChatHandler methods to achieve your chat logic.   


### Notice:

- When chat starts or the `ChatHandler` receives `StateEvent`, `Resumed`, in order to display the chat input field, use the method `enableChatInput` with `super` default implementation or override and set your configurations.   

- `ChatDelegate` provides methods to interact with the UI, like, setting UI components visibility, injecting elements to the UI etc.    
 
- Since Handover is an escalated chat, it will be passed to the 'AccountInfoProvider', to provide a detailed account. In case no extra data should be added to the account, just return the account back.

#### Implementation and usage examples:

```kotlin 
override fun startChat(accountInfo:AccountInfo?) {
    // create and start your custom chat session.
    enableChatInput(true, null)
    // when created:
    injectSystemMessage(SystemStatement("Handover chat started");
    chatDelegate?.injectIncoming(LocalChatElement("Hi from handover", getScope()));
}

override fun endChat(forceClose: Boolean) {
    handleEvent(State, StateEvent(StateEvent.Ended, getScope()))
    enableChatInput(false, null)
    chatStarted = false
}

override fun post(message: ChatStatement){
    chatDelegate?.injectOutgoing(message.apply{
        this.status = StatusOk
    });
    // or for later status update use:
    chatDelegate?.updateStatus(message.getTimestamp(), StatusOk); 
}
```
<details>
<summary> <B>Creating a `Handover` chat channel on Bold360ai console</B> </summary>
Pursue the following sample:    

![](images/Android/handoverChannel.png)

</details>
