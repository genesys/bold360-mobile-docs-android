# Error Event
This article will help you to handle errors of chat controller.

To get error event firstly register to `ChatControllerDelegate` then implement `didFailWithError` method.

>BLDChatErrorType: To get `BLDChatErrorType` options open `BLDError.h` file.

```swift
func didFailWithError(_ error: BLDError!) {
    switch error.type {
    case GeneralErrorType:
        print("GeneralErrorType")
    case BLDChatErrorTypeFailedToStart:
        print("BLDChatErrorTypeFailedToStart")
    case BLDChatErrorTypeFailedToFinish:
        print("BLDChatErrorTypeFailedToFinish")
    case BLDChatErrorTypeFailedToSubmitForm:
        print("BLDChatErrorTypeFailedToSubmitForm")
    default:
        break
    }
}
```

>Note: `BLDError` contains `NSError` object that reflects the relevant error data.

**Code Sample**

[bold360ai samples](https://github.com/bold360ai/bold360ai-mobile-samples)