---
layout: default
title: How it works
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/how-it-works
nav_order: 1
---

# How it works {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
All supporting UI components enables the hosting app to apply some UI changes to the SDKs provided implementations or to completely replace with custom components.   
{: .overview}   

On [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) creation an instance of `ChatUIProvider` can be provided by the hosting App. 
The `ChatUIProvider` defines the SDKs UI components. According to it's definitions and configurations the chat UI component are created when needed.   

In order to change and/or override the SDKs provided implementations and customizations, you need to pass your customed `ChatUIProvider` instance on ChatController creation. 

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

---

## How to configure and override UI components display: 
The ChatUIProvider provides access to the chat ui components. Each component enables changes in 3 ways, which will be combinded.

### Configure   
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

### Customize   
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

### Override
Enable developer to provide his own implementation of the component.   
Overriding can be done mainly in 2 ways, according to the component support:   

Custom implementation
{: .strong-sub-title} 
Providing a custom implementation of the component specific interface, as defined by the component's overrideFactory property type.    
    
> Exp:   
    Provide a new implementation for the feedback view by implementing the `FeedbackUIAdapter` interface.   

```kotlin
class MyFeedbackFactory : FeedbackUIProvider.FeedbackFactory{
    override fun create(context: Context, feedbackDisplayType: Int): FeedbackUIAdapter {
        return new FeedbackViewDummy(context);
    }
}

chatUIProvider.chatElementsUIProvider
                .incomingUIProvider.feedbackUIProvider
                    .overrideFactory = MyFeedbackFactory()
```

Override view resource
{: .strong-sub-title .mt-6}
Providing a different layout resource file, in the form of `ViewInfo` object.   
    
```kotlin
class MyUserInputProvider : ChatUIProvider.ChatUIFactory{
    override fun userInputInfo(): ViewInfo {
        return ViewInfo(R.layout.layout_name, R.id.custom_input_container)
    }
}

chatUIProvider.overrideFactory = MyUserInputProvider();
```
