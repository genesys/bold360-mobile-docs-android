---
layout: default
title: Chat Forms
parent: Chat Configuration
nav_order: 5
# permalink: /docs/chat-configuration/tracking-events/forms
---

# Live chat forms {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The SDK provides un-configurable implementation of the following forms:   
- **PreChat** - Lets the user set some configurations to the up coming chat session, and provide some user details that will be available to the agent once the chat will start.

- **PostChat** - Uses for email setting for chat transcript delivery and as a survey form to give the user the ability to rate and give feedback about the ending chat conduction.

- **Unavailable** - In case the chat can't be performed at the moment, the user can provide his email, to be contacted later.

- **Email** - Uses for submitting an email address, mid-chat, for the chat transcript delivery when the chat ends.
{: .overview}

Each form construct of dynamic list of fields configured on the admin console and set for the account's chat window.  
Each field has properties which defines its type and behavior. Among them: field type, isRequired, and if available, its default/current value.

![]({{'/assets/images/console-chat-forms.png' | relative_url}})
{: .image-70}

---

## How to customize
The SDK provides a way for the hosting App to use self customed form implementations, and display them when needed.

Follow the next steps to use your implementation for some / All available forms.

### 1. Create custom form
Create a custom implementation of the chat form for the forms you would like to override.  

> [Sample code](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/topics/extra/CustomForm.kt)   
    
{: .mt-1}

The custom implementation should use the provided `FormListener`, in order to post results or activate defined actions.   
- `FormListener.onComplete` should be called to submit form data to the SDK.
- `FormListener.onCancel` should be called if the form submission was canceled.
- `FormListener.onLanguageRequest` should be called, if chat language should be changed ([more](#set-chat-language)).
        
{: .mt-8}
### 2. Implement the `FormProvider` interface.
In case an implementation of the FormProvider is available on the ChatController, it will be called for action, once a form should be displayed.    
The method `FormProvider.presentForm(formData: FormData, callback: FormListener)` will be activated, where:
* `FormData` - Provides the data needed to display the form; fields, branded language dependent texts, etc...
* `FormListener` - Results submission callback  

> [Sample code](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/topics/BoldCustomChatForm.kt)   
{: .mt-3}

### 3. Pass FormProvider implementation on `ChatController` build. 
   
```kotlin
ChatController.Builder(context)
        .formProvider(FormProviderSample)
        ...
        .build(account,...)
```

{: .mt-8}
## How to mix form display
In case there's a need to mix custom and SDK provided form display, once the `presentForm` method is triggered, we enable to activate the SDK provided form instead of the custom one by calling <u>formListener.onComplete(**null**)</u>.
   
###### _[checkout sample implementation](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/topics/BoldCustomChatForm.kt)._
{: .no_toc}

---

## Setting extra data
Setting extraData values on the BoldAccount, can be used for **prefilling the prechat form** values.   
the extraData provides some info to the agent about the user, and applies some configurations to the chat.

- _Available extraData keys can be viewed on `SessionInfoKeys`_

```kotlin
// setting extraData to the chat account:
// 1. 
boldAccount.addExtraData(SessionInfoKeys.Department to results.departmentId,
                         SessionInfoKeys.Language to "en-US", ...);

//2.
boldAccount.info.language = ...
```

> _Configured `Language` has preference over `Department`. If the configured department doesn't support the configured language, the user will be directed to a different department which supports his language (If no such available, will be directed to a department which is not dedicated to a specific language)_
{: .overview}

Setting a departmentId on the extraData will designate the chat to this department, if available.    
If a prechat form is enabled for that chat, the requested department will be set as `default value` on the departmets selection field.

---

## Prechat form 

### Set chat language
Live chat language will always defaults to `English`. Configuring alternative language can be done in 2 ways:
1. **[Configure over extraData](#setting-extra-data)** - Chat will be created with the configured language, or defaulted to `English` if something went wrong.

2. **Configure over prechat form**, via language selector field. Dynamically change the language of the current ongoing chat.
    
### Language field
In order to enable users to change the default configured chat language, over the prechat form, the following `Admin console` configurations, should be set.

- Language field should be checked for the accounts Chat window.   
![]({{'/assets/images/language-field-check.png' | relative_url}})
{: .image-70}

- Chat window customizations, should configure other languages, and provide a custom content for each language.   
![]({{'/assets/images/admin-branding-config.png' | relative_url}})
{: .image-70}

The SDK provides the support to change chat language, when SDKs prechat form is in use, and provides the ability to use that support, when the prechat form is customed by the hosting App.

### Language change support for Custom prechat form
When the hosting App provides it's own implementation for the prechat form, and it's time to display that form, `FormProvider.presentForm(FormData, FormListener)` is activated.   
The provided `FormListener` is used for form submission, canceletion and language changes.

In order to activate language change proccess, call:   `FormListener.onLanguageRequest(LanguageChangeRequest, LanguageCallback?)`.
`LanguageChageRequest` holds the selected language code and the current FormData object used to dsplay the prechat form. If all goes well, on the language chage results, an updated FormData instance will be provided, with the selected language branded content.
> Notice: Departments field options may vary from one language to another, depends on the language support defined for each department.

![]({{'/assets/images/language-for-department.png' | relative_url}})   
{: .image-70} 

e.g.
```kotlin
formListener?.onLanguageRequest(
    LanguageChangeRequest(selectedLanguage, formData)) { results ->
        results.error?.let {
            toast(context, "Failed to change language to: $it",
                                                    Toast.LENGTH_LONG)
            // reset language field to previous language: results.language

        } ?: let {
            // update language field value to the selected language
            // update form fields with updated branded labels 
            updateForm(results.formData)
        }
    }
```                    

### Skip Prechat Form 

In order to skip the prechat form, activate `BoldAccount.skipPrechat()` method, on your BoldAccount before chat creation.   

When skipping prechat form, by filling `extraData` details on the account, the chat accepting agent will be exposed to those details.   
The `extraData` can also be used to set the language and department that should be assigned for this chat.

---

## Postchat form
If enabled, will appear when chat ends.
Postchat form has 2 variations:
- Survey form - Introduces some rating and feedback fields, according to admin console configurations.
![]({{'/assets/images/postchat-survey-console.png' | relative_url}})
{: .image-70}

- Email only form - Appears on chat end, when chat survey is disabled and `Send transcript` feature is enabled.

> Disable both features on the console in order to prevent postchat form display.

Postchat submission results are delivered as `PostchatResults` object which introduces chat transcript related properties, like `transcriptEmail`, which holds the user filled email address.
Postchat submission message correlates submission state - feedback only, feedback with transcript email, email only or both.

### Send chat transcript to customer
Transcript delivery feature defined by `Send transcript to customer` configurations, located on the accounts chat window, and will be available only if enabled on the admin console.
![]({{'/assets/images/transcript-console.png' | relative_url}})
{: .image-70}

User can request chat transcript delivery in 2 ways:
{: .strong-sub-title}
- Upon postchat form when chat ends, an `email` field is added to the regular postchat fields (or appears as single field when survey is disabled).

- Mid-chat, upon Email form, triggered by activating the email action button displayed on the [chatbar]({{'/docs/chat-configuration/ui-customization/chat-bar' | relative_url}})

--- 

## Email form 
As mentioned above, this form enables users to set the email address for the chat transcript.

![]({{'/assets/images/email-form.png' | relative_url}})
{: .image-40}

Email form can also be provided by the hosting App, by that, overriding the usage of the SDKs provided form.   

### Submit email on custom form
In order to submit the user email from your custom form, update the email field on the provided FormData.
```kotlin
// update email field data:
formData.chatForm.getFormField(FieldKey.EmailKey).setValue(EMAIL_ADDRESS);

// post the updated Form object over FormListener            
formListener.onComplete(formData.chatForm);
```            

> In case of email submission error, `NRError` will be sent to the hosting App over the regular errors receiving channel, `ChatEventListener.onError` implementation.

---

## Listening to form submission results
Prechat and postchat forms [submission results can be observed]({{'/docs/chat-configuration/tracking-events/events-and-notifications/#subscribing-to-notifications' | relative_url}}) by the hosting App.
Submission results are represented by a `FormResults` object.
- `FormResults.formType` - get the submitted form type (prechat/postchat)
- `FormResults.submitMsg` - Submission message if provided. 
