# Chat Events

## Chat Element Event 

To be triggered by `didReceiveChatElement` follow below steps:

1. Create conversation view (via `ChatController`)

2. Conform to & Set `ChatElementDelegate`
 
```swift
controller.chatElementDelegate = self
```

3. Implement `didReceiveChatElement` under `ChatElementDelegate`

```swift
func didReceive(_ item: StorableChatElement!) {
   // use chat element 
}
``` 
