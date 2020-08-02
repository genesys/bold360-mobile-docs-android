# LiveChatbar component
Once a **Bold live chat** is started a `Chatbar` component will be displayed over the chat.   
The Chatbar contains:
 1. Current live agent's name an avatar.
 2. `End Chat` clickable view to end current live chat.

### How to customize

- Component can be configured or/and overrided via `ChatBarCmpUiProvider`. 
```kotlin
ChatUIProvider(this).apply {
    chatBarCmpUiProvider.configure = { adapter ->
        adapter.configAgentCmp(ChatbarCmpConfig().apply { 
            this.textStyle = StyleConfig(...)
        })
        adapter.configEndCmp(ChatbarCmpConfig().apply {...})
        adapter.setBackground(...)
        adapter
    }
}
```
Find more in [ Customizing UI components](https://github.com/bold360ai/GlobalDocs/wiki/AndroidChatCustomizations).

- Agents avatar placeholder image can be override, by overriding the relevant drawable resource. (agent.png)
- In order to disable the "end chat" display, use the `ConversationSettings`.
    ```kotlin
    ConversationSettings()...
            .disableEndLiveChatSupport()
    ```
