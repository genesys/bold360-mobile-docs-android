# Welcome message
### "On load article" How to configure:
![](https://raw.githubusercontent.com/wiki/bold360ai/GlobalDocs/images/Android/welcome_article_console.png)

Once configured, article id will be accessible on `cnf` response under `onloadQuestionId` property.   
If configured, the welcome article will be displayed on chat start as the first element in the chat. Quick options and channeling of this article will be accessible until first user query.   
`Welcome article` can be configured along side `FAQs`. Both elements will be displayed on chat start.

![](https://raw.githubusercontent.com/wiki/bold360ai/GlobalDocs/images/Android/welcome-and-faqs.png)

### How to customize:
To start a chat with the BOT, a `BotAccount` is passed on the `ChatController` creation.    
Configure the needed customed welcome article id on that `BotAccount` object.
```java
BotAccount account = new BotAccount(...);

// override console configured id or sets one if was not configured 
account.setWelcomeMessage(welcomeMessageId); 

// disable the welcome message if configured in console
account.setWelcomeMessage(BotAccount.None);
```

- BotAccount configured welcome message id, overrides console configuration.

- In order to prevent display of the welcome message, no matter if configured on the console,  set welcome message id to `BotAccount.None`. *(welcome message id `null`, does nothing)*
- If welcome message id was set to an invalid/non-existing article id, no welcome message will be displayed, error will be passed to `ChatEventListener.onError`.   
##
 
> **Notice**, Welcome message is retreived <U>once per chat</U>. If the current chat is a continuance chat, the welcome message won't be retreived again. 