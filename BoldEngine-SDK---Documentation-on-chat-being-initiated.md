# Start chat with bold agent

1. Create the bold session

> **Android**  
  
Chat session creation:   
```
Account.createChat(...)
```
> Chat object (session) will be provided on the `CreateChatListener.onChatCreated` event.   

> **iOS**

```
    _createChatCancelable = [_account createChatSessionWithDelegate:self
                                                           language:@"en-US"
                                                          visitorId:visitorId
                                                     // Make sure to set relevant bool
                                                        skipPreChat:NO
                                                     externalParams:self.extraParams];
```
> Chat object will be provided on `(void)bcAccount:(BCAccount *)account didCreateChat` under `BCCreateChatSessionDelegate`


**Note**: The chat creation may success or fail and you will get 


> Android

```
CreateChatListener.onChatCreated
CreateChatListener.onChatCreateFailed
CreateChatListener.onChatUnavailable
```

> iOS

Implement following delegate:
 
```
BCCreateChatSessionDelegate
```

2. Once chat session was created successfully we need to

> Android

activate `startChat`,
Chat start may success or fail and you will get.

```
ChatStartListener.onChatStarted
ChatStartListener.onChatStartFailed
ChatStartListener.onChatUnavailable
```

> iOS

First make sure session created, when created you should:

```
[self.chatSession.chat addChatStateDelegate:self];
```

Then implement following delegate:

```
BCChatStateDelegate
```

3. At this point we can display a message to the user, notifying his call is in waiting state.

`Waiting for an agent…`

If all succeeded, user can start passing messages to a live agent. The users messages will appear on the agent screen in the incoming message box.

> Notice: If agent answer timeout has been configured in the console, the users chat may be rejected, in case the agent didn’t
> accept the chat within the time limit.

4. In order to validate that the agent accepted the chat

> Android

you need to listen to `ChatListener.onChatUpdated` event.
Once the `Chat.answered` property is no longer `null`, it means that the agent accepted the user chat.

> iOS

Under `BCChatStateDelegate` you have a method:

```
- (void)bcChatDidAccept:(id<BCChat>)chat
```

5. At this point the user can start the actual chat with the agent.

**For more documentation checkout our docs portal:**

> Android

https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_bc_sdk_android_introduction.html

> iOS

https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_bc_sdk_ios_core_integration.html
