---
layout: default
title: Handover Chat
parent: Advanced Topics
nav_order: 10
permalink: /docs/advanced-topics/handover-chat
---

# Handover Chat
{: .no_toc}


## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

--- 

## Overview
**Handing** the control of the chat to a provided custom `HandoverHandler` extension.   
Intended to enable the option to start a chat with a third party provider. 
{: .overview}

## Setting Handover chat escalation
Handover chat is automatically being activated, by the SDK, when chat channel configured with `custom provider` was selected on chat with AI.   

Do the following for a successful Handover chat escalation.

- ### Create Handover escalation channel
    [Create a chat channel](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_use_ww_integration.html) in the Bold360ai admin console, configured with `custom provider`.   
    <sup>For more information see: [How do I define a channeling policy?](https://support.bold360.com/bold360/help/how-do-i-define-channeling-policy)</sup>


- ### Create HandoverHandler
    In order to be able to bridge between your third party chat implementation, and the bold chat SDK, you need to provide an extension of `HandoverHandler` to the ChatController. This handler will connect the user, the chat SDK and the third party  chat in use.

    ```kotlin
    // Custom Handover handler:
    class MyHandover(context:Context) : HandoverHandler(context) {
        override fun startChat(accountInfo:AccountInfo?) {
            // create and start your custom chat session.
            ...
            // pass Started event to the ChatController, and to the App.
            passStateEvent(StateEvent(StateEvent.Started, getScope())
        }

        override fun endChat(forceClose: Boolean) {
            // end your custom chat session.
            ...

            // pass Ended event.    
            passStateEvent(StateEvent(StateEvent.Ended, getScope()))
        }

        override fun post(message: ChatStatement){
            // handle user message - pass it to your third party chat
            ...
            // Add the user message to the chat
            chatDelegate.injectElement(message);
        }
    }

    // Set custon Handover handler to the ChatContoller:
    // on ChatController creation:
    val chatController = ChatController.Builder(context)
                                    .chatHandoverHandler(myHandoverHandler)
                                    ...
                                    .build(account,...)
    // or later:
    chatController.handoverHandler = myHandoverHandler                      
    ```


## How to

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

---

<details>
<summary>Handover chat initiation flow</summary>
add diagram here

</details>