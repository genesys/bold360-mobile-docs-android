
# Chat Departments Fetch
 This article will help you to manage the lifecycle of a chat controller.

The departments list can be fetched any time, without creating `ChatController` instance.

## Fetch Departments

1. Create `Account`.

```swift
// for example create live account.
let liveAccount = LiveAccount()
```

2. Call `fetchDepartments` under `ChatController`.
```swift
ChatController.fetchDepartments(self.createAccount()) { result in
    if let departments = result?.departments {
        self.departments = departments
    }
}
```

## Chat Availability

The availability of the chat can be checked any time, without creating `ChatController` instance.

## Check chat availability

1. Create `Account`.

```swift
// for example create live account.
let liveAccount = LiveAccount()
```

>Note: To check availability for specific **department id** do as below on `LiveAccount`:
 
```swift
liveAccount.extraData?.departmentId = "{DEPARTMENT_ID}"
```

2. Call `checkAvailability` under `ChatController`.
```swift
ChatController.checkAvailability(self.createAccount()) { (isAvailable, reason, error) in
   // Validate no error
   // Check isAvailable
   // If not available check reason
}
```

## Lifecycle Events

| Name            | Description                                                                                                                        |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------|
| ChatPreparing   | Initial state, once the chat creation has started.                                                                                 |
| ChatStarted     | When the chat was successfully started (not yet accepted by agent). At this point the User can start posting messages on the chat. |
| ChatPending     | User chat was assigned to an agent, and waiting for an agent acceptance.                                                           |
| ChatInQueue     | User chat is in the waiting queue, and waiting to be assigned to an agent. In this event the position in the queue is passed.      |
| ChatAccepted    | Agent accepted & Live chat started.                                                                                                |
| ChatEnding      | Live Chat ending.                                                                                                                  |
| ChatEnded       | Live Chat ended.                                                                                                                   |
| ChatUnavailable | Live Chat unavailable.                                                                                                             |


To get lifecycle events firstly register to `ChatControllerDelegate` then implement `didUpdate` method.

>ChatStateEvent: To get `ChatStateEvent` options open `ChatStateEvent.h` file.

```swift
func didUpdateState(_ event: ChatStateEvent!) {
        switch event.state {
        case .preparing:
            print("ChatPreparing")
        case .started:
            print("ChatStarted")
        case .accepted:
            print("ChatAccepted")
        case .ending:
            print("ChatEnding")
        case .ended:
            print("ChatEnded")
        case .unavailable:
            print("ChatUnavailable")
        }
    }
```

### Unavailable Case

>Note: For `unavailable` state you will get `ChatStateUnavailableEvent`

On this case you should use casting to get from `ChatStateEvent` the `ChatStateUnavailableEvent`.

## Lifecycle Methods

### End Chat

To end the current chat you can call the `endChat` method under `ChatController`.

>Note: `endChat` will end current chat only, means, if you started with bot and then moved to live agent when calling `endChat` only the chat with live agent will be ended.

>swift

```swift
self.chatController.endChat()
```

### Terminate Chat

To terminate the chat you can call the `terminate` method under `ChatController`.

>Note: `terminate` will terminate whole chat controller.

>swift

```swift
self.chatController.terminate()
```

### Code Sample

[bold360ai samples](https://github.com/bold360ai/bold360ai-mobile-samples)