# Forms for Android
This article will help you to present your own implementations to chat forms.
> Supported forms are: `preChat`, `postChat`, `unavailable`.

## Using custom forms
In order to change SDK provided implementation, or add implementations that are not available .  
1. Implement the `FormProvider` interface.    
When a form should be displayed, the `presentForm` method will be called. 
   ```kotlin
   interface FormProvider {
       fun presentForm(formData: FormData, callback: FormListener)
   }
   ```  
  * `FormData` - contains all the data needed to present the form: fields, branding map, etc...
  * `FormListener` - The form completion block 
    ```kotlin
    interface FormListener : Completion<Form?> {
        fun onCancel(@FormType formType: String?)
    }
    ```
    - `onComplete` will be called when the form is submitted.    
      As parameter we pass the form to submit, which should contain the user inputs.
      ```kotlin
      formListener.onComplete(formData.form)
      ```
      > **_NOTE:_** If called with `null` value, indicates there is no implementation for the form, and the SDK 
        should display its default form, if available.   
  
    - `onCancel` should be called if the form submission was canceled.
      ```kotlin
      formListener.onCancel(formType) //One of the following constants: PreChatForm / PostChatForm / UnavailabilityForm
      ```
  
2. Pass implementation on `ChatController` build. 
   ```kotlin
   //passing FormProvider 
   ChatController.Builder(getContext()).formProvider(FormProviderImpl)...build(...)
   ```
  

* **[Checkout implementation example](https://github.com/bold360ai/bold360ai-mobile-samples).**

---

## How to skip Prechat Form

In order to skip the prechat, add the following implementation:

```Java
BoldAccount boldAccount = new BoldAccount(bold api key)
boldAccount.skipPrechat();
```

### Adding extra data

When skipping prechat is used, you can provide some details regarding the account by using the accounts `extraData`

```Java
boldAccount.addExtraData(new Pair<>(VisitorDataKeys.Department, department_id));
```
[see available fields here](https://developer.bold360.com/help/EN/Bold360API/Bold360API/c_bc_sdk_ios_core_integration_chat_session.html)