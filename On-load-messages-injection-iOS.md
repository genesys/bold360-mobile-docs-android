# Welcome message

## "On load article" How to configure:
![](https://raw.githubusercontent.com/wiki/bold360ai/GlobalDocs/images/Android/welcome_article_console.png)

Once configured, article id will be accessible on `cnf` response under `onloadQuestionId` property.   
If configured, the welcome article will be displayed on chat start as the first element in the chat. Quick options and channeling of this article will be accessible until first user query.   
`Welcome article` can be configured along side `FAQs`. Both elements will be displayed on chat start.

![](https://raw.githubusercontent.com/wiki/bold360ai/GlobalDocs/images/Android/welcome-and-faqs.png)

### How to customize:
To start a chat with the BOT, a `BotAccount` is passed on the `ChatController` creation.    
Configure the needed custom welcome article id on that `BotAccount` object.
```swift
let account = BotAccount()

// override console configured id or sets one if was not configured 
account.welcomeMessageId = "{WELCOME_MESSAGE_ID}"

// disable the welcome message if configured in console
account.welcomeMessageId = ""
```

- BotAccount configured welcome message id, overrides console configuration.

- In order to prevent display of the welcome message, no matter if configured on the console,  set welcome message id to "" (empty string).
- If welcome message id was set to an invalid/non-existing article id, no welcome message will be displayed.   

 
> **Notice**, Welcome message is retrieved <U>once per chat</U>. If the current chat is a continuance chat, the welcome message won't be retrieved again. 