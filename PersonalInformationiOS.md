# Personal Information

![](https://raw.githubusercontent.com/wiki/bold360ai/bold360ai_ios_sdk/Extra%20Data.png)


```ObjectiveC
@protocol NanorepPersonalInfoHandler <NSObject>

// Invoked before form is being presented
- (void)personalInfoWithExtraData:(NSDictionary *)extraData channel:(NRChanneling *)channel completionHandler:(void(^)(NSDictionary *formData))handler;

// Invoked when user submit
- (void)didFetchExtraData:(ExtraData *)formData;

// On success
- (void)didSubmitForm;

// On cancel
- (void)didCancelForm;

// On failure
- (void)didFailSubmitFormWithError:(NSError *)error;

 // (optional) Enable / Disable Popup confirmation (default is true)
- (BOOL)shouldPresentConfirmationPopupForChannel:(NRChanneling *)channel;

@end
```