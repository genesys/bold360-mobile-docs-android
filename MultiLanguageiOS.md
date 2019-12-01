# How to enable multiple languages

This article will help you to set language from mobile SDK.

Please follow :[Provide a chat window in multiple languages](https://help.bold360.com/help/EN/Bold360/Bold360/t_window_mulitlingual.html).

## Multiple language API usage

1. Create a live account.

```swift
let liveAccount = LiveAccount()
```

2. Set preferred language code.

```swift
liveAccount.languageCode = "fr-FR"
```

```diff
!Remember: Bold360 provides translations in the following languages: 
#English (en-US), Dutch (nl-NL), French (fr-FR), German (de-DE), Italian (it-IT), 
#Spanish (es-ES), Portuguese (pt-BR). 
For all other languages, you must provide your own translations.
```
