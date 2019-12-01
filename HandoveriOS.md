
# Handover Support

This article will help you to add a support to Handover sessions.

Handover is a chat session with a custom provider that is configured and controlled by the customer.
The Handover is being controlled at the app by a provided custom `ChatHandler`.

`Chat handlers` are being used at the SDK in order to separate the chat sessions between different providers such as `Bot` and `Live (bold360ai agent)`.
They handle the user actions and the chat events passed from the chat UI.

## The Chat handlers implement the `ChatHandler` protocol

```swift
class HandOverHandler: NSObject, ChatHandler {
    var delegate: ChatHandlerDelegate!
    
    var chatControllerDelegate: ChatControllerDelegate!
    
    var chatHandlerProvider: ChatHandlerProvider!
    
    // See 'File Upload' doc
    var isFileTransferEnabled: Bool {
        return false
    }
    
    // See 'Autocompletion Support' doc
    var isAutocompleteEnabled: Bool {
        return false
    }
    
    func startChat(_ chatHandlerParams: [String : Any]?) {
        // Present system message
        
        
        // Do the connection to the chat provider
    }
    
    func endChat() {
        
    }
    
    func postStatement(_ statement: StorableChatElement) {
        
        // Configure the bubble
        statement.configuration = self.chatHandlerProvider.configuration(for: .OutgoingElement)
        self.delegate.presentStatement(statement)
        
        // Updated the double "V" sign for read notification
        self.perform(#selector(HandOverHandler.updateBubble(statement:)), with: statement, afterDelay: 3)
        
        // Just for testing you can cancel the handover by typing stop
        if statement.text == "Stop" {
            self.chatHandlerProvider.didEndChat(self)
        }
    }
    
    func didStartTyping(_ isTyping: Bool) {
        
    }
    
    func handleClickedLink(_ link: URL!) {
        
    }
    
    func handleEvent(_ eventParams: [AnyHashable : Any]!) {
        
    }
    
    @objc func updateBubble(statement: StorableChatElement) {
        self.delegate.update(StatementStatus.Pending, element: statement)
    }
}
```

## Handover handler implementation

As noted above, the Handover is being controlled from a class that implements the `ChatHandler` protocol.

In order to add the implemented class to the SDK, add the next at the ChatController:

1. Create new class that implements `ChatHandler` as described above.

```swift
// `HandOverHandler` was created above.
var handOver = HandOverHandler()
```

2. Then in relevant place like `viewDidLoad` do:

```swift
chatController.handOver = self.handOver
```

