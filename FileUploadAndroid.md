# Add File Upload support
This article will help you to configure your disired file upload mechanism.

The SDK provides an upload mechanism, but enables you to use your own mechanism.

## 1.  Define your file upload trigger. 
- **Use SDK trigger.**  
The SDK provides an upload trigger which will be positioned inside the input field. Trigger icon can be changed via ui provider configuration. 
    ```java
    chatUIProvider.getUserInputUIProvider().setUploadImage(...)
    ```
- **Use your own trigger.**   
To do that, disable the display of the SDK trriger. <sub>(By default set to `true`)</sub>
    ```java
    chatUIProvider.getUserInputUIProvider().showUpload(false)
    ```

## 2. Define your file upload mechanisem 
- **Use SDKs file uploader**
When user selects the file to upload use `chatController.uploadFile` method, to upload the file via SDK uploader. Upload indication will be added to the chat automatically.
A `FileUploadInfo` object, should be created for every file that will be uploaded. 
Results will be passed over the callback
    ```kotlin
    //... user selected the file to upload
    val fileInfo = FileUploadInfo() 
    // OR 
    val fileInfo = FileUploadInfo(bytes:ByteArray)

    fileInfo.apply{
        type = ... // as defined in @FileType
        name = ... // can differ from the actual file name
        filePath = ... // actual selected file path
        content = file.readBytes()... // if was not provided on the constructor
    }

    chatController.uploadFile(fileInfo) { uploadResults ->
        //.... got UploadResults and do whatever 
        uploadsResults.error?.run{
            Log.e(TAG, "Got an error on ${uploadResults.data.name} file upload: ${uploadsResults.error}")
            ...
        }
    } 
    ```

- **Use your own file uploader**
When user selects the file to upload use your uploader to upload the selected file.   
**In order to have the upload indication in the chat, after upload completed you should pass event to the `chatController` with the upload results.**
    ```kotlin
    //... user selected the file to upload

    MyUploader.uploadFile(...){
        //.... do upload stuff

        // pass results to the SDK 
        val uploadResults = UploadResults(FileUploadInfo?, NRError?)
        chatController.handleEvent(Upload, new UploadEvent(uploadResult));
    }
    ```

## Extra

### Checking Upload feature availability:

In order to check if the file upload feature is available for the current scope, use `chatController.isEnable(ChatFeatures)` method.

```kotlin
chatController.isEnabled(ChatFeatures.FileUpload);
```


### Listening to upload notifications
One can listen to files upload events via the `ChatController`, by registering to the available uploads notifications he needs.

```kotlin
chatController.subscribeNotifications(notifiableImpl:Notifiable, 
                    Notifications.UploadEnd, Notifications.UploadStart, 
                    Notifications.UploadProgress, Notifications.UploadFailed)

// Notifiable implementation (notifiableImpl):
{
    ...

    override fun onNotify(notification: Notification, dispatcher: DispatchContinuation) {
        
        when (notification.notification) {
            Notifications.UploadStart,
            Notifications.UploadEnd,
            Notifications.UploadFailed -> {
                (notification as UploadNotification).apply {
                    // uploadInfo:FileUploadInfo member is available
                    ...
                }
            }

            Notifications.UploadProgress -> {
                (notification as ProgressNotification).apply {
                    // uploadInfo:FileUploadInfo member is available
                    // progress:Int member is also available
                    ...
                }
            }
        }
    }
}
```

### Customizations:
-  **Uploads progress indication**
    - <U>**Using SDK provided progress indication**</u> - enables customizations of bar location and other visual properties, as declared in `UploadsCmpAdapter` interface.
        ```kotlin
        // display uploads summary bar as floating component which also displays 
        // notifications of start and stop of uploads
        chatUIProvider.uploadsCmpUIProvider.configure = { adapter:UploadsCmpAdapter-> 
            adapter.apply{
                gravity(Gravity.CENTER);
                layoutGravity(Gravity.TOP);
                isFloating  = true;
                showNotifications = true;
                notificationsGravity(Gravity.END);
                notificationsTextStyle(StyleConfig(14, context.resources.getColor(R.color.colorTextLight)))
            }
                
            adapter
        }
        ```
    - <u>**Using overriding customed component**</u> - One can create a customed uploads progress component and display it while uploads progress.
        ```kotlin
        chatUIProvider.uploadsCmpUIProvider.overrideFactory = 
                object: UploadsbarCmpUIProvider.UploadFactory {
                    override fun create(context: Context): UploadsCmpAdapter {
                        val myComponent = .... // create customed component 
                        return myComponent
                    }
                }
        ```
        In order to notify the SDK that the component done with the uploads display and should be removed, pass `CmpEvent` to the `ChatController`:
        ```kotlin
        chatController.handleEvent(CmpEvent.EventName,
                            CmpEvent(ComponentType.UploadsStripCmp,
                                        CmpEvent.Idle))
        ```

        If configurations were defined in `UploadsbarCmpUIProvider` if the customed view implements the `UploadsCmpAdapter` methods, its UI will be configured accordingly.    

- **Upload outgoing bubble** 
    - File type images can be changed by supplying new drawable resources in app resources with the following ids: picture_ico, default_ico, excel_ico, archive_ico.
    - bubble can be configured, as with other chat bubbles, by overriding the default configure method on `UploadElementUIProvider`
        ```kotlin
        ChatUIProvider.chatElementsUIProvider.uploadUIProvider.configure = { adapter ->

            adapter.apply {
                setBackground(new ColorDrawable(Color.RED));
                setTextStyle(new StyleConfig(18, Color.GREEN, 
                            getTypeface(getContext(), "great_vibes.otf")));
                setLinkTextColor(Color.GREEN);
                ...

                adapter
            }
        }  
        ```
        
