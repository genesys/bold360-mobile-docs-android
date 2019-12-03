> This article will help you to add a support to Handover sessions.

## Introduction

Handover is a chat session with a custom provider that is configured and controlled by the customer.
The Handover is being controlled at the app by a provided custom `ChatHandler`.

`Chat handlers` are being used at the SDK in order to separate the chat sessions between different providers such as `Bot` and `Bold`.
They handle the user actions and the chat events passed from the chat UI.

### The Chat handlers implement the `ChatUIHandler` interface

- *`fun setChatDelegate(delegate: ChatDelegate)`* - sets the handler's chat delegate (which is the ui interface instance)

- *`fun setListener(listener: EventListener?)`* - Sets the SDK listener which is used to pass events from the handler back to the SDK

- *`fun startChat(accountInfo: AccountInfo?)`* - Preform actions when the chat has been started.
    The AccountInfo contains data that has been retrieved from the server.
    usage example:

```java
     @Override
    public void startChat(@Nullable AccountInfo accountInfo) {
        if (accountInfo != null) {
            byte[] bytes = accountInfo.getInfo();
            handlerConfiguration = new String(bytes);
        }

        chatDelegate.injectSystem(new SystemStatement("Started Chat with Handover provider, the handover data is: " + handlerConfiguration, getScope()));
        chatDelegate.injectIncoming(new IncomingStatement("Hi from handover", getScope()));
    }
```

- *`fun endChat(forceClose: Boolean)`* - Commit actions when the chat ends (before the handler is being destructed)
    usage example:

```java
    @Override
    public void endChat(boolean forceClose) {
        StateEvent endEvent = new StateEvent(Ended, getScope());
        listener.handleEvent(endEvent.getType(), endEvent);
    }
```

- *`fun post(message: ChatStatement)`* - Here we get the message that the user typed at the sdk's input
    field.
    usage example:

```java
    @Override
    public void post(@NotNull ChatStatement message){
        chatDelegate.injectOutgoing(message);
        chatDelegate.updateStatus(message.getTimestamp(), StatusOk); // can be delayed by the handover provider
    }
```

- *`fun handleEvent(name:String, event: Event)`* - here the handler receives events from the SDK. It can preform actions before before passing them to the event listener.
    usage example:

    ```java
    @Override
        public void handleEvent(@NotNull String name, @NotNull Event event) 

            switch (name) {
                case State:
                {...}
                break;

                case Message:
                {...}
                break;

                case UserAction:
                {...}
                break;

                case Error:
                {...}
                break;
            }

            // If there is any post event function, invoke it after the event handling
            Function0 postEvent = event.getPostEvent();
            if (postEvent != null) {
                postEvent.invoke();
            }
        }
    ```

## Handover handler implementation

As noted above, the Handover is being controlled from a class that implements the `ChatUIHandler` interface.

In order to add the implemented class to the SDK, add the next at the ChatController builder:

```java
    ChatController.Builder chatBuilder = new ChatController.Builder(getContext())
            ...
            .chatHandoverHandler(customHandoverHandler);
            ...
```

## Set a handover channel at the BoldAI console

In order to apply a Handover channel for the account, follow the next channel example:

![](images/Android/handoverChannel.png)
