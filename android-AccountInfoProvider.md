## Account Info Provider

Enables the app to provide session data and configurations, and get updates on account changes.

Implementation should be passed on `ChatController` creation.
```kotlin
val chatController = ChatController.Builder(context)
                                .accountProvider(accountInfoProvider)
                                ...
                                .build(account,...)
```

### Why do i need to implement
- To be able to set data and configurations for chat session creation, when **escalating from ai chat**.
- To be able to receive account updates. like: `chatId` and `visitorId` creation.

### How set extra data for chat session
_Each account has an `info:SessionInfo` property. With this property you can provide extra data <sub>(see `SessionInfoKeys`)</sub> and configurations <sub>(see `SessionInfoConfigKeys`)</sub>, that will be used in chat session creation._

```kotlin
//Kotlin:
boldAccount.info.addExtraData(SessionInfoKeys.FirstName to firstName,
                SessionInfoKeys.LastName to lastName,
                SessionInfoKeys.Email to email,
                SessionInfoKeys.Phone to phoneNumber, ...)

botAccount.info.addConfigurations(SessionInfoConfigKeys.SkipPrechat to true)                

OR

boldAccount.info.department(departmentId)

botAccount.info.welcomeMessage(articleId)

////////////////////////////

//java:
boldAccount.getInfo().addExtraData(new Pair(SessionInfoKeys.FirstName,firstName),...)

OR

LiveSession.setDepartment(boldAccount.getInfo())

LiveSession.setLanguage(boldAccount.getInfo(), "fr-FR")
```




