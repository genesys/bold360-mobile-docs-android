# Personal Information

## Article example for balance

`The balance of your account ending with [[ACCOUNT.ID]] is [[ACCOUNT.CURRENCY]] {{getAccountBalance([[ACCOUNT.ID]],[[ACCOUNT.TYPE]])}} [[=ACCOUNT_OPTIONS]]`


## Init chat with `Balance` scenario

```java
    BotAccount account = new BotAccount();
    account.setAccount("ACCOUNT");
    account.setApiKey("API KEY");
    account.setKnowledgeBase("KNOWLEDGEBASE");
    account.setEntities(new String[]{"USER_ACCOUNTS", "BALANCE"});
```
In order to provide personal information and entities to according to the SDK's providers requests:  
1. Implement the `EntitiesProvider` interface.    
When **personal information** is requested by the SDK, it should be provided by the next `provide` method: 
```java
public void provide(@NotNull PersonalInfoRequest personalInfoRequest, @NotNull PersonalInfoRequest.Callback callback)
```
When **missing entities** are requested by the SDK, it should be provided by the next `provide` method: 
```java
public void provide(@NonNull ArrayList<String> entities, @NonNull Completion<ArrayList<Entity>> onReady)
```
  
2. Pass implementation on `ChatController` build.
   ```kotlin
   //passing EntitiesProvider 
   ChatController.Builder(getContext()).entitiesProvider(EntitiesProviderImpl)...build(...)
   ```
---
## Implementation example

### The `EntitiesProvider` interface Implementation

```java
  @Override
        public void provide(@NonNull ArrayList<String> entities, @NonNull Completion<ArrayList<Entity>> onReady) {
            NRConversationMissingEntities missingEntities = new NRConversationMissingEntities();
            for (String missingEntity : entities) {
                switch (missingEntity) {
                    case "SUBSCRIBERS":
                        missingEntities.addEntity(createEntity(missingEntity));
                        break;
                }
            }

            onReady.onComplete(new ArrayList<Entity>(missingEntities.getEntities()));
        }

        @Override
        public void provide(@NotNull PersonalInfoRequest personalInfoRequest, @NotNull PersonalInfoRequest.Callback callback) {
            String balance = "0";
            switch (personalInfoRequest.getId()) {
                case "BALANCE":
                   balance = String.format("%10.2f$", Math.random() * 10000);
                    callback.onInfoReady(balance, null);
                    return;
            }

            callback.onInfoReady(balance +"$", null);
        }
```

![](images/Android/personalInfo.png)