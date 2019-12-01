# Forms for iOS
This article will help you to present your own implementations to chat forms.
> Supported forms are: `preChat`, `postChat`, `unavailable`.

## Custom Form Implementation

1. Implement `ChatControllerDelegate`.

```swift
// Declare delegate
ViewController: ChatControllerDelegate
// Set controller delegate
self.chatController.delegate = self
```

2. Implement `shouldPresent` delegate method.

>`BrandedForm` - Contains all the data needed to present the form: fields, branding map, etc...

>`completionHandler` - The form completion block. 

```swift
func shouldPresent(_ form: BrandedForm!, handler completionHandler: (((UIViewController & BoldForm)?) -> Void)!) {
    if (completionHandler != nil) {
        if form.form?.type == BCFormTypePostChat {
            let postVC = self.storyboard?.instantiateViewController(withIdentifier: "postChat") as! PostChatViewController
            postVC.form = form
            completionHandler(postVC)
        } else if (form.form?.type == BCFormTypeUnavailable) {
            let unavailableVC = self.storyboard?.instantiateViewController(withIdentifier: "unavailable") as! UnavailableViewController
            unavailableVC.form = form
            completionHandler(unavailableVC)
        } else {
            completionHandler(nil)
        }
    }
}
```

### Form Lifecycle

To dismiss form, end user should submit form or tap on back button.

>Note: bold360ai SDK must be notified about the dismiss reason.

1. Implement `BoldForm`.

```swift
// Declare delegate
FormName: BoldForm
// Implement property
var delegate: BoldFormDelegate! {
    get {
        return formDelegate
    }
    set(delegate) {
        formDelegate = delegate
    }
}
```

>Form Submission 

*Please Notify When Form Submit Button Was Tapped*

```swift
self.delegate.submitForm(brandedForm)
```

>Form Cancelation

*Please Notify When Back Button Was Tapped To Dismiss Form.* 

```swift
self.delegate.didDismissForm(self)
```

## How to skip Prechat Form

In order to skip the prechat, add the following implementation:

```swift
let account = LiveAccount()
account.apiKey = "{YOUR_API_KEY}"
account.shouldDisablePreChat = skipPrechat.isOn
```

### Adding extra data

When skipping prechat is used, you can provide some details regarding the account by using the accounts `extraData`

```swift
account.extraData.name = "{NAME}"
account.extraData.department = "{DEPARTMENT_ID}"

or

account.extraData.setExtraParams(["department":"{DEPARTMENT_ID}","name": "{NAME}", "address": "{ADDRESS}"])
```


[see available fields here](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_bc_sdk_ios_core_integration_chat_session.html)
 
### Code Sample

[bold360ai forms samples](https://github.com/bold360ai/bold360-mobile-samples-ios/tree/master/iOS/FormsBasicSample)