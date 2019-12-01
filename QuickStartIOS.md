# Quick Start

In this section, you'll learn how to build a basic conversation application using the bold360ai SDK for iOS.

## Create the Project  

### Set Up a Project in Xcode  

1. Open Xcode and click **start new Xcode Project**:
        ![](images/iOS/conversation/newProj.png)

2. Next, select **Single View Application** and click **Next**:
        ![](images/iOS/conversation/singleView.png)

3. In the dialog screen displayed, enter the relevant details:
        ![](images/iOS/conversation/projDetails.png)

### Import the Native SDK

Go to the desired file (e.g., `ViewController.swift`) and add the line below:

>swift

```swift
import Bold360AI
```

### Setup Chat Controller

>swift

```swift
    var chatController: ChatController!
    var chatControllerDelegate: ChatControllerDelegate!
    var chatHandlerProvider: ChatHandlerProvider!
    var chatViewController: UIViewController!
```

### Setup Live Chat

>swift

```swift
// setup Live Chat
extension ViewController {
    @IBAction func setupBoldChat(_ sender: Any) {
        // 1. create account & set
        let account = LiveAccount()
        account.apiKey = {API_KEY}
        self.chatController = ChatController(account: account)
        // 2.  set controller delegate
        self.chatController.delegate = self
    }
}
```

> Note: `LiveAccount` is managed by forms (preChat, postChat, unavailable).
>Only preChat form contains default SDK implementation, means postChat and unavailable forms should be implemented by the application.
>Make sure to read [chat lifecycle doc](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_sdk_combined_ios_adv_chat_lifecycle.html) and register to relevant states (e.g `unavailable` state).

### Setup Bot Chat

>swift

```swift
// Setup Bot Chat
extension ViewController {
    @IBAction func setupBotChat(_ sender: Any) {
        // 1. create account & set
        let account = self.createAccount()
        self.chatController = ChatController(account: account)
        // 2.  set controller delegate
        self.chatController.delegate = self
    }
    
    func createAccount() -> BotAccount {
        let account = BotAccount()
        account.account = "ACCOUNT"
        account.knowledgeBase = "KNOWLEDGE_BASE"
        account.apiKey = "API_KEY"
        return account;
    }
}
```

#### Use Context

Context is a key-value parameter, so when you create the `BotAccount` object you can set NSDictionary which contains the related context.

``` swift
// Using the context:
botAccount.context = ["someKey": "someValue"]
```

#### Configure Welcome Message Id

When you create the `BotAccount` object you can set welcome message id.

```swift
botAccount.welcomeMessageId = "{WELCOME_MESSAGE_ID}"
```

>To Disable Welcome Message 

```swift
botAccount.welcomeMessageId = WelcomeMsgIdNone
```

### Add Chat View Controller by Implementing Delegate Methods  

>swift

```swift
extension ViewController: ChatControllerDelegate {
    func didFailLoadChatWithError(_ error: Error!) {
        print(error.localizedDescription)
    }
    
    func shouldPresentChatViewController(_ viewController: UINavigationController!) {
        // 4. present modally/ as child view controller over your view controller. 
       <YOUR_VIEW_CONTROLLER>.present(viewController, animated: false, completion: nil)
    }
}
```

### Code Sample

[bold360ai samples](https://github.com/bold360ai/bold360-mobile-samples-ios)