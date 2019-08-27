## .homeStoryboard

To use a storyboard that is not `Main.storyboard` as the application's entry point, simply set this custom Storyboard as `.homeStoryboard` inside of the `withOptions` in `Cely.setup(_:)`.


```swift

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
        .homeStoryboard: UIStoryboard(name: "NonMain", bundle: nil)
    ])

    return true
}

```
