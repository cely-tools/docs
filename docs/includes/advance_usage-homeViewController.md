## .homeViewController

To use a `ViewController` that is not the **Initial View Controller** in `Main.storyboard`, simply set this custom ViewController as `.homeViewController` inside of the `withOptions` in `Cely.setup(_:)`.


```swift

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
        .homeViewController: UIStoryboard(name: "NonMain", bundle: nil).instantiateViewController(withIdentifier: "HomeViewController")
    ])

    return true
}

```
