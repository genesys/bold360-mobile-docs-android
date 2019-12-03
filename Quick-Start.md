## **1. Prerequisites**

* Java 8 or higher
* Gradle 4.6 or higher
* Android API 19 or higher
* Android Studio

## **2. Installation dependencies**

   In the `build.gradle` file, add this line to the dependencies:

```java
   compile 'xxx'
```  

## **3. Starting chat with bot**

**a) Create BotAccount object and add the chat fragment to the activity.**

```java
BotAccount botAccount = new BotAccount(api_key, account_name,
                        knowledge_base, server, context);
```  

BotAccount constructor receives the following parameters: account_name, knowledge_base, api_key and server.

**api_key** (mandatory) is the key that is being sent to the server for authorization.

**account_name** (mandatory) is the name used to identify the account (for example: Jio).

**knowledge_base** (mandatory) is used to identify the knowledge base we are trying to access.

**server** is the server the requests are being sent to

**context** (optional) is used to filter results depending on user's parameters

**b) Create ChatController object.**

 chatController object is used for interacting between the application and the chat SDK.

see the example below

**c) Add the chat fragment to the activity.**

In order to receive and add the chat fragment, we need to implement the ChatLoadedListener interface and pass its implementation to the build() method.

```java
new ChatController.Builder(Context).build(botAccount, new ChatLoadedListener() {
      @Override
      public void onComplete(ChatLoadResponse result) {
        Fragment chatFragment = null;
          if (result.getError() == null) {
              chatFragment = result.getFragment();
          }

          getFragmentManager().beginTransaction()
                  .replace(R.id.content_main, chatFragment, CONVERSATION_FRAGMENT_TAG)
                  .addToBackStack(null)
                  .commit();
      });
```
