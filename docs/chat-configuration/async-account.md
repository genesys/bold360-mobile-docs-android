---
layout: default
title: AsyncAccount
parent: Setting Account
grand_parent: Chat Configuration
nav_order: 3
permalink: /docs/chat-configuration/setting-account/async-account
---

# AsyncAccount

    
```kotlin
val account = AsyncAccount(API_KEY, APPLICATION_ID)
```

In order to provide a specific user id and info, (by that, relate all chats with the same id, to the same user), add `UserInfo` to the account creation. If `userId` is not provided, one will be automatically generated. 

```kotlin
// kotlin:
val account = AsyncAccount(API_KEY, APPLICATION_ID).apply {
    info.userInfo = UserInfo(USER_ID).apply { // Mandatory
        firstName = FIRST_NAME // optional
        lastName = LAST_NAME // optional
        email = EMAIL_ADDRESS // optional
        phoneNumber = PHONE_NUMBER // optional
    }
}
```
```java
// Java:
UserInfo userInfo = new UserInfo(USER_ID);
userInfo.setFirstName(FIRST_NAME);
userInfo.setLastName(LAST_NAME);
userInfo.setEmail(EMAIL_ADDRESS);
userInfo.setPhoneNumber(PHONE_NUMBER);

AsyncAccount account = new AsyncAccount(API_KEY, APPLICATION_ID);
AsyncSession.setUserInfo(account.getInfo(), userInfo)
```
> Async messages response wait timeout can be configured on the accounts SessionInfo as well:
`info.ackTimeout = MS_Value`. If was not set,the default value will be used, 6000ms. 


