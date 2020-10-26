---
layout: default
title: Chat Availability
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 2
# permalink: /docs/chat-configuration/tracking-events/chat-availability
---

# Chat Availability {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Chat availability check

1. Create an `Account`.

2. Call `ChatAvailability.checkAvailability` as follows:
    ```kotlin
    ChatAvailability.checkAvailability(account, callback = object : ChatAvailability.Callback {
        override fun onComplete(result: ChatAvailability.AvailabilityResult) {
            // Validate no error
            // Check isAvailable and check the UnavailabilityReason if not 
        }
    })
    ```

    > `ChatAvailability.AvailabilityResult` provides the check parameters (apiKey, departmentId, etc) as well as the execution results 

- ### How to check chat availability on a <U>specific department</U>? 
  Use `ChatAvailability.checkAvailability` call as before, just add the departmentId.   
    
  ```kotlin
  ChatAvailability.checkAvailability(account, departmentId, callback = object : ChatAvailability.Callback {
      override fun onComplete(result: ChatAvailability.AvailabilityResult) {
          // do your wonders...  
      }
  })
  ```

  > **<U>Important:</U>** In order to get availability status of the different departments in your organization, [make sure your api access key is not configured]({{'/assets/images/bold-console-apikey.png' | relative_url}}) to work with a specific department.

## How to retrieve available departments list
1. Create an `Account`.

2. Call `ChatAvailability.availableDepartments` as follows:
    ```kotlin
    ChatAvailability.availableDepartments(account, callback = object : ChatAvailability.DepartmentsCallback {
        override fun onComplete(result: ChatAvailability.DepartmentsResult) {
            // Validate no error
            // use the list
        }
    })
    ```

> `ChatAvailability.DepartmentsResult` if no errors occurred will contain a list of `Department` items. Each contains name, id and language of the department.

---

### How to start a live chat with a specific department
  1. create a `BoldAccount`
  ```
  val account = BoldAccount(apiKey)
  ```

  2. _Add the department id to the account's extraData_
  ```kotlin
  account.addExtraData(VisitorDataKeys.Department to departmentId)
  ```

   3. _Start chat with the account_
   ```kotlin
   ChatController.Builder(context)...
                    .build(account,...)
   ```

   > - When the chat configured to start with a prechat form, the provided department will be set as default option in the department form field.   
   > - If there is no need to display the prechat form, configure the account to skip prechat, that way the chat will start with one of the department agents, if available.


