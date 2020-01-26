# Chat Engagement

The SDK supplies tools for the __**`app`**__ to engage and manipulate conversation flow.
Statements can be injected to the chat with or without chat element indication, and as various response sources.

## Statements injection

* **Using the ChatController**, this is the easiest way to inject statements.

```kotlin
chatController.post(Statement);
```

* **Using ConversationFragment**

``` java
((NRConversationFragment) fragment).getChatDelegate().injectElement(ChatElementFactory.create(Statement);
```

___

**The `Statement` objects shown above are instances of `ChatStatement`, such as `OutgoingStatement`, `IncomingStatement` and `SystemStatement`.**
 ___

### Statement Scopes

Statements request/response has a `scope` definition.   
By default `scope = StatementScope.NanoBotScope()`.   
According to scope definition reposted failed requests, are identified properly.   
Scopes can also be used to define, different behavior or different look, to the requests/responses under a certain scope.   
_Currently_, scopes can only indicate if a request/response was done as [`Handover`](ChatHandover) or not.

### Available Scopes:

- **_NanoBotScope_** - Restricted to SDK use only. Marks requests that are passed to the bot, and responses that are returned from it.
- **HandoverScope** - Indicate request/response under `Handover session`.
- **BoldScope** - Indicate request/response under `Bold session`.
- **BoldScope** - Indicate request/response under `Async session`.