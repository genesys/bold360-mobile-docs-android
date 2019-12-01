# Enable file upload.

This article will help you to support file upload.

## 1. Enable file transfer in the admin console.

1. Open [https://admin.bold360.com/](https://admin.bold360.com/) and log in.
2. Go to CHANNELS -> Chat -> Chat Windows -> Choose relevant window -> File Transfer section.
3. Check `Enable` and choose: `Agent to customer` , `Customer to agent`.

## 2. Add File Upload Button

### 2.1. Use Default File Upload Button

Implement `didClickUploadFile` optional `ChatControllerDelegate` method.

```swift
extension FileUploadDemoViewController: ChatControllerDelegate {
    func didClickUploadFile() {
        // present file picker
     } 
}
```

### 2.2.  Add Custom File Upload Button

a. Listen to relevant chat state by implementing `didUpdateState` optional `ChatControllerDelegate` method.
b. Validate file transfer enabled on bold admin console.
>Note: If not valid upload will fail.
c. Add custom button.

```swift
func didUpdateState(_ event: ChatStateEvent!) {
    switch event.state {
    ///a. Listen to relevant chat state
    case .pending:
        /// b. Validate file transfer enabled on bold admin console.
        if(self.chatController.isFileTransferEnabled) {
            /// c. Add custom button.
            DispatchQueue.main.async {
                self.uploadBtn.backgroundColor = .blue
                self.uploadBtn.setTitle("Upload File", for: .normal)
                self.uploadBtn.frame.size = CGSize(width: 150, height: 70)
                self.uploadBtn.center = (self.navigationController?.visibleViewController?.view.center)!
                self.uploadBtn.addTarget(self, action: #selector(self.uploadFile), for: .touchUpInside)
                self.navigationController?.visibleViewController?.view.addSubview(self.uploadBtn)
            }
        }
        break
    default:
        break
    }
}
```

## 3. Handle File Upload Button Tap

Once button tapped, add file selection view.

## 4. Once file was picked create upload request.

```swift
let request = UploadRequest()
request.fileName = (resources.first!).originalFilename
request.fileType = .picture
request.fileData = data
```

## 5. Once upload request created start upload file process.

### 5.1. Call handle function on ChatController with upload file information.
### 5.2. You can get progress values, to show upload progress bar.

```swift
/// 5. Once upload request created start upload file process.
self.chatController.uploadFile(request, progress: { (progress) in
    /// 5.1. You can get progress values, to show upload progress bar.
    print("application file upload progress -> %.5f", progress)
}) { (info: FileUploadInfo!) in
    /// 5.2. Call handle function on ChatController with upload file information.
    self.uploadBtn.removeFromSuperview()
    self.chatController.handle(BoldEvent.fileUploaded(info))
}
```

### Limitation

File upload is supported only on live chat.