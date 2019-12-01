# Incoming chat component

## Readmore indication <sub><sup>(configurable only)</sub></sup>
- `readmore` indication view is configurable via `ReadmoreAdapter`, as follows:   

```kotlin
ChatUIProvider(context).apply {
    
    chatElementsUIProvider.incomingUIProvider.readmoreUIProvider.configure = { 
        adapter:ReadmoreAdapter -> 
        
        adapter.apply {
            alignReadmore(...)
            setReadmoreStyle(...)
            ...
        }
    }        
}
```
- `readmore` indication text, is a string resource (`R.string.read_more`), and can be override by the integrating app.

##
### Persistent options
Incoming bot response can have persistent options. Those options will not dissappear after user selection.   
#### How to customize
This kind of incoming element can be customized by overriding default implementation of the PersistentOptionsUIProvider.
- The options style can be configured by `PersistentOptionsUIProvider.optionsStyleConfig`   
- The wrapping bubble can be customized by `PersistentOptionsUIProvider.contentStyleConfig`
  ```kotlin
  ChatUIProvider(context).apply {
      
      chatElementsUIProvider.incomingUIProvider.persistentOptionsUIProvider.apply {
          // customize options text style:
          optionsStyleConfig = StyleConfig(...)
          
          // customize the wrapping bubble look:
          contentStyleConfig = { adapter -> 
              adapter.textBackground(...)
              
              // do more adjustments
          }
      }    
  }
  ```

- Options look can also be customized by override as follows:
```kotlin
ChatUIProvider(context).apply {
    
    chatElementsUIProvider.incomingUIProvider.persistentOptionsUIProvider.overrideFactory = 
                object : UIInfoFactory {
                    override fun info(): ViewInfo {
                        /* return your own ViewInfo object - 
                                which defines the layout resource for the options */
                    }
                }
}
```