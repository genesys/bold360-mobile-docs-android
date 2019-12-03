> This article will take you through the lifecycle of chat controller.

## Lifecycle Events

To get lifecycle events implement the interface `ChatEventListener`, and pass it on the `ChatController` builder.

```kotlin
fun onChatStateChanged(stateEvent: StateEvent)
```

## Lifecycle Events

| Name      | Description                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Preparing | Once the chat creation has started, `StateEvent` will be passed on method `onChatStateChangedwith` state `StateEvent.Preparing`                                                                                               |
| Started   | When the chat was successfully started (not yet accepted by agent) a `StateEvent.Started` event will be passed.,At this point the User can start posting messages on the chat, those will be received by the accepting agent. |
| Pending   | User chat was assigned to an agent, and waiting for an agent acceptance.                                                                                                                                                      |
| InQueue   | User chat is in the waiting queue, and waiting to be assigned to an agent.,In this event the position in the queue is passed.                                                                                                 |
| Accepted  | When an agent accepted the users chat initiation, an event will be passed `StateEvent.Accepted`. Agent will receive all previous posted messages.                                                                             |
| Ending    | When the chat started ending process an event `StateEvent.Ending` will be passed.                                                                                                                                             |
| Ended     | When the chat was ended an event `StateEvent.Ended` will be passed.                                                                                                                                                           |

### There are 2 extra events that will be passed to `onChatStateChanged`:

| Name               | Description                                                                                                                                                                |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ChatWindowLoaded   | will be triggered once the chat fragment was loaded and ready to display chat elements and receive injections.                                                             |
| ChatWindowDetached | Will be triggered once the chat window should be removed from its activity. (triggered when all active chats were terminated or when user _"backed"_ from the chat screen) |

**To end current chat** use `chatcontroller.endChat` method. If you don't want to display the postChat form activate this method with forceClose = true, `chatController.endChat(true)`   

**To end All chats at once** use `chatController.terminateChat` method. this will end all active chats.
an event will be passed to `onChatStateChanged` `StateEvent.ChatWindowDetached` indicating that chat fragment should be removed since we're no longer has an active chat.   
   
> **_Notice:_** Since app inserted the chat fragment to its activity with `ChatLoadedListener` on the chatController build, it should be the one to remove it. so this event tells the developer, that its time to remove the chat fragment from the activity.

### States handling example:

```java
    @Override
    public void onChatStateChanged(@NotNull StateEvent stateEvent) {

        switch (stateEvent.getState()) {

            case Preparing:
                // do something
                break;

            case Started:
                // do something
                break;

            case Unavailable:
                // do something
                break;

            case Ended:
                // do something
                break;

            case ChatWindowLoaded:
                // do something
                break;

            case ChatWindowDetached:
                // removing chat fragment from its activity:
                FragmentManager fragmentManager = getSupportFragmentManager();
                if (fragmentManager != null) {
                    fragmentManager.popBackStack(CONVERSATION_FRAGMENT_TAG, FragmentManager.POP_BACK_STACK_INCLUSIVE);
                }
                break;
        }
    }
```