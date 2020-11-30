---
layout: default
title: Chat Forms
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 6
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
- PreChat - Enabling the user to provide some extra data that will be available to the agent once the chat will start. 
- PostChat - Left the user give some feedback about the ending chat.
- Unavailable (email) - In case the chat can't be performed at the moment, the user can provide his email, to be contacted later.
{: .overview}

Once a form should be presented to the user, the Bold BE provides a list of fields that should be displayed on that form.   
The form fields are configured on the admin console, under the accounts chat window.   
Each field has properties which defines its look and behavior, among them: field type, isRequired, and if available, its current value.

![]({{'/assets/images/console-chat-forms.png' | relative_url}})
{: .image-70}

---

## How to customize
The SDK provides a way for the embedding App to use self customed form implementations, and display them when needed.

1. ### Create customed form
    Create a customed implementation of the chat forms.

    ```kotlin
    class FormDummy : Fragment() {

    private var formData: FormData? = null
    private var formListener: WeakReference<FormListener>? = null

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        // go over formData.fields list and construct the form views ...

        submit.setOnClickListener {

            // go over the form fields view and set the corresponding 
            // value to each field in formData.fields
            ...

            // pass results back to the SDK:
            formListener?.get()?.onComplete(formData?.chatForm)
        }
    }

    override fun onStop() {
    // in case the form was was canceled, user doesn't submit form
    // the onCancel should be activated on the formListener.  
        if (isRemoving && !isSubmitted) {
            listener?.get()?.onCancel(data?.formType)
        }

        super.onStop()
    }
    ```
  > - `FormListener.onComplete` should be called to submit form data to the SDK.
  > - `onCancel` should be called if the form submission was canceled.
        

2. ### Implement the `FormProvider` interface.
    
   In case an implementation of the FormProvider is available on the ChatController, it will be called for action, once a form should be displayed.    
  The method `FormProvider.presentForm(formData: FormData, callback: FormListener)` will be activated.
   * `FormData` - Provides the data needed to display the form; fields, branding map, etc...
   * `FormListener` - Results submission callback

   ```kotlin
    class FormProviderSample: FormProvider {
        
        override fun presentForm(formData: FormData, @NonNull callback: FormListener) {

            // Create the customed form implementation:
            val fragment = FormDummy.create(formData, callback)

            // add fragment to the activity to be displayed.
            supportFragmentManager
                .beginTransaction()
                .replace(R.id.chat_container, fragment, FORM_DUMMY_FRAGMENT_TAG)
                .addToBackStack(FORM_DUMMY_FRAGMENT_TAG)
                .commitAllowingStateLoss()
        }
    }
    ``` 
     
3. ### Pass FormProvider implementation on `ChatController` build. 
   
   ```kotlin
   ChatController.Builder(context)
           .formProvider(FormProviderSample)
           ...
           .build(account,...)
   ```

## How to mix form display
In case there's a need to mix custom and SDK provided form display, once the `presentForm` method is triggered, we enable to activate the SDK provided form instead of the custom one by calling <u>formListener.onComplete(**null**)</u>.
   
###### _[checkout sample implementation](https://github.com/bold360ai/bold360ai-mobile-samples)._
{: .no_toc}

---

## Setting extra data
Setting extraData values on the BoldAccount, can be used for **prefilling the prechat form** values.   
the extraData provides some info to the agent about the user, and applies some configurations to the chat.

> _Available extraData keys can be viewed on `SessionInfoKeys`_

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

---

## Set chat language
Live chat language will always defaults to `English`. Configuring alternative language can be done in 2 ways:
1. **[Configure over extraData](#setting-extra-data)** - Chat will be created with the configured language, or defaulted to `English` if something went wrong.

2. **Configure over prechat form**, via language selector field. Dynamically change the language of the current ongoing chat.
    
### Prechat form - Language field
In order to enable the user, change the chat language over the prechat form, the following `Admin console` configurations, should be set.

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

Exp:
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


---

## Skip Prechat Form 

In order to skip the prechat form, activate `BoldAccount.skipPrechat()` method, on your BoldAccount before chat creation.   

When skipping prechat form, by filling `extraData` details on the account, the chat accepting agent will be exposed to those details.   
The `extraData` can also be used to set the language and department that should be assigned for this chat.


