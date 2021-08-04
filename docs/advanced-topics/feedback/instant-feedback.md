---
layout: default
title: Instant Feedback
parent: Feedback
grand_parent: Advanced Topics
nav_order: 1
# permalink: /docs/advanced-topics/feedback/instant-feedback
---

# Instant Feedback<sub>(Feedback Per Article)</sub>  {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview  
Response specific feedback, provides the tool to review specific content quality and relevancy.   
When enabled, every AI response will be followed with a feedback UI element, as long as it valid for feedback submission.
{: .overview }

---

## Instant feedback Configurations
The feedback UI component is configurable, by bold360ai console configurations, and by SDK defined costomizations.

### Admin console configurations
{: .mb-4 }
- **Feature availability**   
    {: .fs-4 .fw-300 }
  Feedback feature can be truned on/off on the admin console per widget specific configurations to your account and knowledge base.

  <details close markdown="block">
  <summary>Feedback status settings</summary>
  ![]({{'/assets/images/instant-feedback-console.png' | relative_url}})
  {: .image-70 }
  </details> {: .mb-6 }

- **Feedback display type** 
    {: .fs-4 .fw-300 }
    There are 2 predefined display options for the feedback component, that can be selected on the admin console, textual and iconic.

    <details close markdown="block">
    <summary>Feedback display type settings</summary>
    ![]({{'/assets/images/feedback-display-type.png' | relative_url}})
    {: .image-70 }
    </details> {: .mb-4 }

    The Bold SDK provides default implementations for both types.


    Iconic feedback:
    {: .blue-sm-tl}

    |![]({{'/assets/images/iconic-idle-feedback.png' | relative_url}})||
    |---|---|
    |![]({{'/assets/images/iconic-negative-feedback.png' | relative_url}})|![]({{'/assets/images/iconic-positive-feedback.png' | relative_url}})|
    {: .table-trans}

    
    Textual feedback:
    {: .blue-sm-tl}

    |![]({{'/assets/images/textual-idle-feedback.png' | relative_url}})||
    |---|---|
    |![]({{'/assets/images/textual-negative-readmore-feedback.png' | relative_url}})|![]({{'/assets/images/textual-positive-feedback.png' | relative_url}})|
    {: .table-trans}

- Textual configurations
  On the admin console you can configure the messages content that are presented to the user over feedback flow. 

  ![]({{'/assets/images/feedback-texts.png' | relative_url}})
  {: .image-70}
 
---

### SDK configurations
{: .mb-4}
Using the SDKs internal UI implementation is not configurable. We provide the option to integrate a custom feedback UI implementation which will override the internal SDKs implementation.

📚 How to change internal feedback icons
{: .strong-sub-title .mt-6}
This can be done by overriding the icons drawable resources by the hosting application.
Override the following drawables:
- feedback_negative_icon
- feedback_negative_selected_icon
- feedback_positive_icon
- feedback_positive_selected_icon

Custom Feedback UI
{: .strong-sub-title .mt-6}
1. Create your custom feedback view. Make it implement `FeedbackUIAdapter` interface.
    ```kotlin
    class CustomIconFeedbackView : LineaLayout, FeedbackUIAdapter {
        ...
    }

    class CustomTextFeedbackView : LineaLayout, FeedbackUIAdapter {
        ...
    }
    ```
2. Create implementation for `FeedbackFactory`, that creates your custom feedback view.
    ```kotlin
    class MyCustomFeedbackFactory : FeedbackFactory{
        override fun create(context: Context, feedbackDisplayType: Int): FeedbackUIAdapter {
            return when (feedbackDisplayType) {
                IconicFeedback -> CustomIconFeedbackView(context)
                else -> CustomTextFeedbackView(context)
            }
        }
    }
    ```
3. Set feedback provider factory to point to your implementation, in the `ChatUIProvider`. 
    ```kotlin
    val chatUIProvider = ChatUIProvider().apply{
        chatElementsUIProvider.incomingUIProvider.feedbackUIProvider.overrideFactory = MyCustomFeedbackFactory
    }

    val chatController = ChatController.Builder(context).apply {
                            chatUIProvider(chatUIProvider)
                            ....
                        }.build(account...)

    ```

## Accessibility support
Feedback buttons are read to the user when the accessibility service is turned on.   
The feedback texts are configured to the accessibility read as defined in the admin console for the [positive and negative feedback texts]({{'/assets/images/instant-feedback-text.png' | relative_url}}).
If feedback texts were not configured on the admin console, the SDK's string resources will be used as defined on `R.string.instant_feedback_no` for negative and `R.string.instant_feedback_yes` for positive.

When a feedback type is selected by the user, some messages and actions will be available to the user, depends on his selection. The messages and optional actions are handled by the accessibility service as other messages in the chat.

