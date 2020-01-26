## [`.loginViewController`](/api/constants/#enum-celyoptions)

To use your own login, set the custom Login ViewController as `.loginViewController` inside of the `withOptions` in `Cely.setup(_:)`.

```swift

// iOS 13 | Swift 5.0 | Xcode 11.0

Cely.setup(with: window, forModel: User(), requiredProperties: [.token], withOptions: [
    .loginViewController: UIHostingController(rootView: LoginContentView())
])

```
