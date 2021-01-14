---
layout: default
title: How it works
parent: UI Customization
grand_parent: Chat Configuration 
# permalink: /docs/chat-configuration/ui-customization/how-it-works
nav_order: 1
---

# Customizing chat UI basics {{site.data.vars.need-review}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
All customizable UI components enable the hosting app to apply some UI changes to the SDKs default implementations or replace them completely by custom components.   
{: .overview}   

This can be done by providing a custumized instance of `ChatUIProvider` upon [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) creation by the hosting App. 
The `ChatUIProvider` defines the SDKs UI components, and their default display. When a UI component is created, it will be customized and configured according to its specific definitions as configured on the `ChatUIProvider`.   
{: .overview}

Setting a customized `ChatUIProvider` instance:
{: .strong-sub-title .mb-0}
```kotlin
// 1. create ChatUIProvider instance:
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

// 2. Pass the instance to the ChatController Builder
ChatController.Builder(context)
                    .chatUIProvider(uiProvider)
                    ...
                    .build(...) 
```

---

## Customization methods: 
The `ChatUIProvider` provides access to the chat UI components configurations and creation. 
The UI components supports one or more of the following customization methods:
{: .overview}

### Component customization on creation  
- <a id="object-configure"> Some UI components supports a **Configuration object**. 
{: .strong-sub-title}
This object lists the customizable properties available for that component.
In order to customize those properties, access the relevant config object from the `ChatUIProvider` instance, and set their values.

e.g., Set the text style for the chat input field
{: .eg-class}
```kotlin
ChatUIProvider(context).apply {
    chatInputUIProvider.uiConfig.inputStyleConfig = StyleConfig(...)
}   
```

{: .mt-6}
- <a id="adapter-configure"> Some UI components supports a **Configuration adapter**. 
{: .strong-sub-title}
The adapter defines the customization methods available for this component.
In order to change the set of customization methods and their parameters, which are activated on the component, once it was created,
access the relevant UI component definition from the `ChatUIProvider` instance, and override its `configure` method implementation.

e.g., Set the avatar image to incoming messages
{: .eg-class}
```kotlin
ChatUIProvider(context).apply {
    chatElementsUIProvider.incomingUIProvider.configure = { adapter ->
        adapter.setAvatar(Context.resources.getDrawable(R.drawable.my_avatar))
    }
}
```

{: .mt-8}
### Component customization on data update   
Dynamic customization support, where configured customization is applied to a specific component when it gets its data.
Components with dynamic customization support, defines a **Configuration adapter** and an **ElementModel**. 
`Configuration adapter` - defines a set of customization methods which can be applied to the component. 
`ElementModel` - defines the data that will be displayed by this component.
The `customize` method, which is available for this component, on the `ChatUIProvider`, will be activated with those parameters, when the customization should be applied to it.   
In order to change the set of customization methods and their parameters, access the relevant UI component definition from the `ChatUIProvider` instance, and override its `customize` method implementation.

e.g., Applying specific design properties if the incoming message came from a live agent 
{: .eg-class}
```kotlin
ChatUIProvider(context).apply {
    chatElementsUIProvider.incomingUIProvider.customize = { adapter: BubbleContentUIAdapter, element: IncomingElementModel? ->
        // The provided data model can be used to apply data dependant customizations.  
        element?.takeIf(it.elemScope.isLive)?.let {
            adapter.apply {
                setAvatar(ContextCompat.getDrawable(context, R.drawable.speaker_on))
                setTextStyle(StyleConfig(10, Color.WHITE))
                setBackground(ColorDrawable(Color.RED))
            }
        }
        adapter
    }
}
```

{: .mt-8}
### Component customization by override
Defines a custom component or component layout which will replace the SDK default implementation.   

- Some UI components supports override by Custom implementation
{: .strong-sub-title} 
Providing a custom component and implementing a specific interface, that will enable integration of the custom component with the SDK.
The interface that should be implemented is defined by the component's overrideFactory property type.    
    
e.g., Custom feedback component, which should implement the `FeedbackUIAdapter` interface.   
{: .eg-class}
```kotlin
class MyFeedbackFactory : FeedbackUIProvider.FeedbackFactory{
    override fun create(context: Context, feedbackDisplayType: Int): FeedbackUIAdapter {
        return new FeedbackViewDummy(context);
    }
}
ChatUIProvider(context).apply{
    chatElementsUIProvider.incomingUIProvider
            .feedbackUIProvider.overrideFactory = MyFeedbackFactory()
}
```

- Some UI components support override of the view layout resource
{: .strong-sub-title .mt-6}
Providing a custom layout resource file for the component layout, using `ViewInfo` object.   
> `ViewInfo` object defines the layout resource id and the id of the active view within the layout. 
    
e.g., Replace the System message component layout
{: .eg-class}
```kotlin
class CustomSystemMessageFactory : SystemElementUIProvider.SystemFactory{
    override fun info(): ViewInfo {
        return ViewInfo(R.layout.layout_name, R.id.message_textview)
    }
}
ChatUIProvider(context).apply{
    chatElementsUIProvider.systemUIProvider.overrideFactory = CustomSystemMessageFactory()
}
```
