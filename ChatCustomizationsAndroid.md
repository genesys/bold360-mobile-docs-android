###### <sup> This article will help you customize initial chat view UI, and instruct you how to change UI on runtime according to live data. </sup> 
##
## Customizing UI components 
ChatController expects to have an instance of `ChatUIProvider`. This class defines the chat UI components; Creates and customize the views used by the SDK.   
In order to change and override provided SDK implementations and customizations, one need to pass his own customed `ChatUIProvider` instance on `ChatController.Builder`. 
```kotlin
    // creating the provider instance:
    ChatUIProvider uiProvider = ChatUIProvider(context).apply{

        // change the chat screen background:
        setChatBackground(context.getResources().getDrawable(R.drawable.bkg_bots));

        // changing default properties for the incoming bubble views:
        chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
            adapter.apply {
                setAvatar(...)
                setTextStyle(...)
            }
        }

        // overriding feedback view factory:
        chatElementsUIProvider.incomingUIProvider
                .feedbackUIProvider.overrideFactory = MyFeedbackFactory()


        ... // apply more customizations
    }

    ChatController.Builder(context)
                        .chatUIProvider(uiProvider)
                        ...
                        .build(...) 
```
#### How to configure and override UI components display: 
The ChatUIProvider provides access to the chat ui components. Each component enables changes in 3 ways, which will be combinded.

- #### Configure -   
  Provides the ability to configure ui settings on a configuration adapter. Those values will be applied for all components of this type.   
  In order to change the provided SDK configurations, change the `configure` method implementation.
  ```kotlin
  // method signature:
  open var configure: ((Adapter) -> Adapter)

  // applying custom implementation
  chatUIProvider.chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
      adapter.setAvatar(Context.resources.getDrawable(R.drawable.my_avatar))
  }
  ```
- #### Customize -   
  Provides the ability to dynamically change ui settings, which were configured by `configure`, on the adapter, according to a data model that will be displayed by the component. Those changes will applied only to the specific component.  
  ```kotlin
  // method signature:
  open var customize: ((Adapter, DataElement?) -> Adapter)?

  // applying custom implementation
  chatUIProvider.chatElementsUIProvider.incomingUIProvider.customize = { adapter, element ->
      element?.takeIf { it.elemScope.isLive() }?.run {
          adapter.setAvatar(Context.resources.getDrawable(R.drawable.agent_avatar))
      }
  }
  ```

- #### Override -   
    Enable developer to provide his own implementation of the component.   
    ##### Overriding is enabled in 2 main ways:
    1. By providing custom views that implement a specific interface.    
        ###### <U>sample:</U> Provide a new implementation for the feedback view by implementing the `FeedbackUIAdapter` interface.
        ```kotlin
        class MyFeedbackFactory : FeedbackUIProvider.FeedbackFactory{
            override fun create(context: Context, feedbackDisplayType: Int): FeedbackUIAdapter {
                return new FeedbackViewDummy(context);
            }
        }

        chatUIProvider.chatElementsUIProvider.incomingUIProvider.feedbackUIProvider.overrideFactory = MyFeedbackFactory()
        ```

    2. By providing a different layout resource file, in the form of `ViewInfo` object.   
        ```kotlin
        class MyUserInputProvider : ChatUIProvider.ChatUIFactory{
            override fun userInputInfo(): ViewInfo {
                return ViewInfo(R.layout.layout_name, R.id.custom_input_container)
            }
        }

        chatUIProvider.overrideFactory = MyUserInputProvider();
        ```