# Supported Continuity Features

This section describes the different operations you can perform with the continuity service.

* **Update** - updates continuity item 
* **Fetch** - fetches parameters stored in update method.
  > For Example: on start chat can store chatID.

## Known Limitations

* Continuity persistent storage is currently not provided by the SDK, <br />
  it should be done by the using app. 

## Overview
When implementing the `ContinuityProvider`, on chat start the `fetchContinuity` method will be called.
Using `updateContinuityInfo` method can be used to store or expose data of each chat. e.g. Retrieving the `chatID` of the previews chat.
 
### Simple Flow  

## Usage  

### General API Notes  

The following classes/interfaces are the public API for continuity management:

* **`ChatController`** - Use this class to set `ContinuityProvider`.

### Basic Implementation

1. Create conversation view (via `ChatController`)

2. Conform to & Set `ContinuityProvider`
 
```swift
controller.continuityProvider = self
```

3. Implement `ContinuityProvider` Functions

```swift
func updateContinuityInfo(_ params: [String : String]!) {
        params.forEach { (key, value) in
            UserDefaults.standard.set(value, forKey: key)
        }
        UserDefaults.standard.synchronize()
}
    
func fetchContinuity(forKey key: String!, handler: ((String?) -> Void)!) {
        handler(UserDefaults.standard.value(forKey: key) as? String)
}
``` 
4. Present Chat viewController.

