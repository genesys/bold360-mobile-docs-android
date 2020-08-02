## System message constomization
Currently system messages look can be changed only by `override`.
##### How to customize:
  - ##### override layout resouce:
    Create your own layout resources with the following names:   
    - Regular messages: system_message_regular (inner textview should keep SDK known id: message_textview)
    - Removable messages: system_message_removable (inner textview should keep SDK known id: message_textview)

  - ##### Override SystemUIProvider factory:
    1. Implement SystemFactory: 
        - override `removableSystemInfo` method, for changing the layout for the removable system messages (appears for a specific state and than are removed from the chat)
        - override `info` method for changing the regular messages display.
    2. Set `SystemUIProvider.overrideFactory` with your implementation.

    ```kotlin
    class MySystemMessageFactory: SystemFactory{
        override fun info():ViewInfo{
            return ....
        }
        override fun removableSystemInfo():ViewInfo{
            return ....
        }
    }
    
    val chatUI = ChatUIProvider(context).apply {
        this.chatElementsUIProvider.systemUIProvider.overrideFactory = mySystemMessageFactory
    }

    ChatController.Builder(context).apply{
            //... do some initiations
            chatUIProvider(chatUI)
        }
    ```