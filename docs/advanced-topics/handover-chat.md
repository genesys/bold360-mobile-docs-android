---
layout: default
title: Handover Chat
parent: Advanced Topics
nav_order: 10
permalink: /docs/advanced-topics/handover-chat
---

# Handover Chat
**Handing** the control of the chat to a provided custom `HandoverHandler` extension.  

### How to activate
Handover chat is automatically being activated, by the SDK, when chat channel configured with `custom provider` was selected on chat with AI.   
Do the following for a successful Handover chat escalation.

- [Create a chat channel](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_use_ww_integration.html) in the Bold360ai admin console, configured with `custom provider`.   
<sup>For more information see: [How do I define a channeling policy?](https://support.bold360.com/bold360/help/how-do-i-define-channeling-policy)</sup>

- Set your customed `HandoverHandler` extension instance to the `ChatController`.

    ```kotlin
    // on ChatController creation:
    val chatController = ChatController.Builder(context)
                                    .chatHandoverHandler(myHandoverHandler)
                                    ...
                                    .build(account,...)
    // or later:
    chatController.handoverHandler = myHandoverHandler                      
    ```

### HandoverHandler extension
1. Extend `HandoverHandler`.   
2. Override ChatHandler methods to achieve your chat logic.  
    
    <details open markdown="block">
    <summary> Code example:</summary>

    ```kotlin 
    class CustomedHandover(context:Context) : HandoverHnadler(context){

        override fun startChat(accountInfo:AccountInfo?) {
            // create and start your custom chat session.

            // when created:
            injectSystemMessage(SystemStatement("Handover chat started");
            injectElement(LocalChatElement("Hi from handover", getScope()));
        }

        override fun endChat(forceClose: Boolean) {
            handleEvent(State, StateEvent(StateEvent.Ended, getScope()))
            enableChatInput(false, null)
            chatStarted = false
        }

        override fun post(message: ChatStatement){
            chatDelegate.injectElement(message.apply{
                this.status = StatusOk
            });
            // or for later status update use:
            updateStatus(message.getTimestamp(), StatusOk); 
        }
        ....

    }
    ```
    </details> 


### How to

- ### Inject and update chat elements   
    HandoverHandler base class provides various methods, like: `injectElement`, `updateElement`, `storeElement`, etc, that can be used while the handover chat is in progress.   
    Base class implementations also make sure elements changes are passed to the `ChatElementListener` (History updates)

- ### Display and enable the chat input field
    On chat start or/and on state `StateEvent.Resumed`, your custom handler should enable the chat input field, in order to let the user type messages. This can be done by activating the method `enableChatInput`. Override this method, if you need to configure different behavior to the field other than the default provided by its super.

- ### Control chat UI components
    The HandoverHandler has access to a `ChatDelegate` implementation, which provides access to the chat fragments UI components, the chat elements and other abilities.   

    **Exp: Controling AgentTyping UI component visibility state:**
    ```kotlin
    // show AgentTyping:
    chatDelegate?.updateCmp(ComponentType.LiveTypingCmp, data = null)

    // hide AgentTyping:
    chatDelegate?.removeCmp(ComponentType.LiveTypingCmp)
    ```

- ### Adding extra details and configurations for chat creation
    Handover chat is created by the hosting app. It is provided by a `HandoverAccount` that may contain some configurations needed for the chat.   
    Before the chat starts, the app will be triggered to [provide](./account-info-provider#account-provide) the account needed for the chat, at this point, details can be added to the the account [SessionInfo](./account-info-provider#session-info) property.   
    If no extra details are needed, the account should be passed as is.


