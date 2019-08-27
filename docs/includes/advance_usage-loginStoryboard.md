## .loginStoryboard

To use your own login screen, simply create a storyboard that has your custom `LoginViewController` as the **Initial View Controller**. Next, set this custom Login Storyboard as `.loginStoryboard` inside of the `withOptions` in `Cely.setup(_:)`.


```swift

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
        .loginStoryboard: UIStoryboard(name: "MyCustomLogin", bundle: nil)
    ])

    return true
}

```
