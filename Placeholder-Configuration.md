# Placeholder Configuration Support
Chat SDK supports placeholder text for the search input field.

### How to configure
Placeholder configurations should be set by `ChatController.ChatConfiguration.SearchViewConfiguration.PlaceholderConfiguration`
```swift
let placeholderConfig = PlaceholderConfiguration()
placeholderConfig.text = "Placeholder text"
placeholderConfig.textColor = UIColor.red
placeholderConfig.font = UIFont(name: "Times New Roman", size: 20)
chatController.viewConfiguration.searchViewConfig.placeholderConfiguration = placeholderConfig
```

## 
 **!! Notice:** _Client side settings overrides console settings._   
   To configure console settings:
![](./images/iOS/searchView-placeholder-configuration.png)
##