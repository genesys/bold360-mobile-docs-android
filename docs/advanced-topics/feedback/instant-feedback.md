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
  ![](/assets/instant-feedback-console.png)
  {: image-70 }
  </details> {: .mb-6 }

- **Feedback display type** 
    {: .fs-4 .fw-300 }
    There are 2 predefined display options for the feedback component, that can be selected on the admin console, textual and iconic.

    <details close markdown="block">
    <summary>Feedback display type settings</summary>
    ![](/assets/feedback-display-type.pn)
    {: image-70 }
    </details> {: .mb-4 }

    The Bold SDK provides default implementations for both types.


    Iconic feedback:
    {: .blue-sm-tl}
    |![](/assets/iconic-idle-feedback.png)||
    |---|---|
    |![](/assets/iconic-negative-feedback.png)|![](/assets/iconic-positive-feedback.png)|
    {: .table-trans}

    
    Textual feedback:
    {: .blue-sm-tl}
    |![](/assets/textual-idle-feedback.png)||
    |---|---|
    |![](/assets/textual-negative-readmore-feedback.png)|![](/assets/textual-positive-feedback.png)|
    {: .table-trans}

- Textual configurations
On the admin console you can configure the messages content that are presented to the user over feedback flow. 

![](/assets/feedback-texts.png)
{: image-70}
 
---

### SDK configurations
The instant feedback display can be override by the app, via `ChatUIProvider`.
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