# Bold AI User Context

Bold AI context is a key-value parameter, so when you create the `AccountParams` object you can set `NSDictionary` which contains the related context (in this case the `AccounParams` is being created in the `AppDelegate` class).

``` swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: (UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        let accountParams: AccountParams = AccountParams()
        accountParams.account = "nanorep"
        accountParams.knowledgeBase = "English"
        
        // Using the context:
        accountParams.nanorepContext = ["someKey": "someValue"]
        NanoRep.shared().prepare(with: accountParams)
        return true
    }
```