---
layout: default
title: Error Handling
parent: Tracking Events
grand_parent: Chat Configuration
nav_order: 4
---

# Error Handling
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
When an error occure during SDK functionality, the error is passed to the hosting app, unless can be managed internaly.   
Errors can raise due to connection problems, missuse, and many other reasons. Errors are passed to the hosting app via `ChatEventListener.onError` implementation.   
Error events passes objects of NRError which defines the error that occured. The NRError consist of `code`, `reason`, `description`, and sometimes contains some extra `data`.
{: .overview}

---

## Subscribe to error events 
On `ChatController` creation pass an implementation of `ChatEventListener`, which overrides the `onError` method. 

```kotlin
ChatController.Builder(context)
        .chatEventListener(object : ChatEventListener{
            override fun onError(error: NRError) {
                // handle error
            }
            ....
        })
        ...
```

---

## Error Codes

Following we list possible errors and their reasons.

### General Errors

- `GeneralError` - Global error, specific data is not needed.
- `AccessControlError` - requested access (to a critical system resource such as the file system or the network) is denied
- `EmptyError` - Expected data was not delivered
- `ExceptionError` - Error due to exception
- `ServerError`
- `ClientError`
- `ConnectionException`
- `TimeOutError`
- `NotFoundError`
- `ForbiddenError`
- `MissingRequestParamsError`

{: .mt-7}
### Chat with AI Errors

- `ConversationCreationError` - unable to create conversation   
  possible reasons:
  - `NotEnabled` - Chat with the provided account is not supported
  - `EmptyResponse` - empty/null response
  - `NotAvailable` - connection problem
  - `IllegalStateError` - account type is not recognized/other
{: .mt-4}
- `StatementError` - unable to send statement   
  possible reasons:
  - `ConversationNotFound` - connection problem
  - `MissingRequestParamsError` - indicates missing parameters on a certain operation. Like, missing account data for upload or required missing entities data.
{: .mt-4}
- `ConfigurationsError` - unable to fetch configuration

{: .mt-7}
### Live/Messaging chat Errors

- `ChatCommunicationError` - There was a communication problem with the chat server.
- `TemporaryDisconnection` - Connection disconnected, the system tries to reconnect.
- `InvalidChatMessage` - A message parse failure.
- `FormSubmissionError`
- `LanguageChangeError`
{: .mt-4}
- `PERMISSION_ERROR`(404)   
  possible reasons:
  - `IllegalStateError` - Chat state is wrong for the requested action.
  - Internet access was not granted
{: .mt-4}
- `ChatStartError`(503)   
  possible reason:
  - `MalformedAccessKey` - access key issue
