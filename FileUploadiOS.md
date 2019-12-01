# Enable file upload.

This article will help you to support file upload.

File transfer should be enabled in the admin console.

## Checking Upload feature availability

To check if file upload enabled on admin console do as below.

```swift
self.chatController.isFileTransferEnabled
```

>This indicator can be used when attaching custom upload button.

### File Upload Implementation

#### 1. Implement `didClickUploadFile` optional `ChatControllerDelegate` function.

```swift
extension FileUploadDemoViewController: ChatControllerDelegate {
    func didClickUploadFile() {
        // present file picker
     } 
}
```

#### 2. Once file was picked start upload file process.

  >2.1. SDK **default upload**.

```swift
let request = UploadRequest()
request.fileName = _{file_name}_
request.fileType = _{file_type}_ // choose relevant file data from `UploadFileType`.
request.fileData = _{file_data}_ // Send file data as byte array.
                
self.chatController.uploadFile(request, progress: { (progress) in
    print("application file upload progress -> %.5f", progress)
}) { (info: FileUploadInfo!) in
    // Go to step 3.
}
```

   >2.2. **custom upload**.

```swift
// 1. Add you own upload process implementation.
...

// 2. Once file upload process ended create `FileUploadInfo` object with upload result.

let infoFile = FileUploadInfo()

//if file upload was successful
infoFile.fileDescription = "file was uploaded, http://link.to.file"

//else if you had a failure 
let error = NSError(domain: "", code: 0, userInfo: [NSLocalizedDescriptionKey:"file failed to upload"])
infoFile.error = error

```

#### 3. Call `handle` function on `ChatController` with upload file info.

```swift
// send the info file result.
self.chatController.handle(BoldEvent.fileUploaded(infoFile))
```

### Limitation

File upload is supported only on live chat.