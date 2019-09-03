## .loginViewController

To use your own login, set the custom Login ViewController as `.loginViewController` inside of the `withOptions` in `Cely.setup(_:)`.


```swift

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
        .loginViewController: UIStoryboard(name: "Main", bundle: nil).instantiateViewController(withIdentifier: "LoginViewController")
    ])

    return true
}

```
