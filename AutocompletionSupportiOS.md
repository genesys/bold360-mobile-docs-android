# Autocomplete support

## Standalone Bot autocomplete component

The SDK provides a standalone bot autocomplete component, `SearchViewController`.

> This component supports self state restoring, like on rotation mode changes.

## How to use

1. Override `prepare` to get `SearchViewController`.

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // Get the new view controller using segue.destination.
    // Pass the selected object to the new view controller.
    guard let controller = segue.destination as? SearchViewController else {
        return
    }
}
```
2. Update suggestions open direction by setting `autoCompleteOnTop`.

```swift
// foe example
 controller.autoCompleteOnTop = true
```

3. Set the controller account.

```swift
controller.account = self.createAccount()

// Set relevant account params
func createAccount() -> Account {
        let account = BotAccount()
        account.account = ""
        account.knowledgeBase = ""
        return account
}
```

4. Set view configurations on controller.

```swift
// For example
controller.view.layer.borderColor = UIColor.red.cgColor
controller.view.layer.borderWidth = 2.0
```

5. Register to controller's delegate `SearchViewControllerDelegate`.

```swift
controller.delegate = self
```

> Note: at the end `prepare` method should look like below:

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // Get the new view controller using segue.destination.
    // Pass the selected object to the new view controller.
    guard let controller = segue.destination as? SearchViewController else {
        return
    }
    if segue.identifier == "Top" {
        controller.autoCompleteOnTop = true
    }
    controller.delegate = self
    controller.account = self.account
    controller.view.layer.borderColor = UIColor.red.cgColor
    controller.view.layer.borderWidth = 2.0
}
```

> Please implement `SearchViewControllerDelegate`.

```swift
extension AutoCompleteViewController: SearchViewControllerDelegate {
    func didSelectSuggestion(_ suggestion: [String : NSCopying]) {
    }
    
    func didSubmitText(_ text: String) {
    }
    
    func speechRecognitionDidFail(with status: NRSpeechRecognizerAuthorizationStatus) {
    }
    
    func updateHeight(with diffHeight: CGFloat) {
    }
}
```

## How to configure

Autocomplete configurations should be set by `AutoCompleteConfiguration` under `SearchViewConfiguration`.

```swift
let config = SearchViewConfiguration()
let autoCompleteConfig = AutoCompleteConfiguration()
autoCompleteConfig.textColor = UIColor.red
config.autoCompleteConfiguration = autoCompleteConfig
controller.configuration = config
```



