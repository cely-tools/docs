<!-- # Usage

- Getting Started
  - Diagram the ish out
- Cely.setup
- CelyUser
  - recommended pattern
- CelyAnimator
- CelyStyle -->


## Usage

### CelyOptions
#### Handle Login Credentials
Now how do we get the `username` and `password` from Cely's default LoginViewController? It's easy, just pass in a completion block for the `.loginCompletionBlock`. Check out [`CelyOptions`](#Cely.CelyOptions) for more info.

```swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User(), requiredProperties: [.token], withOptions: [
        .loginCompletionBlock: { (username: String, password: String) in
            if username == "asdf" && password == "asdf" {
                Cely.save(username, forKey: "username")
                Cely.save("FAKETOKEN:\(username)\(password)", forKey: "token", securely: true)
                Cely.changeStatus(to: .loggedIn)
            }
        }
    ])

    return true
}
```
#### Customize Default Login Screen

Create an object that conforms to the [`CelyStyle`](#Cely.CelyStyle) protocol and set it to `.loginStyle` inside of the [`CelyOptions`](#Cely.CelyOptions) dictionary when calling Cely's [`setup(_:)`](#Cely.setup) method.

```swift
// LoginStyles.swift
struct CottonCandy: CelyStyle {
    func backgroundColor() -> UIColor {
        return UIColor(red: 86/255, green: 203/255, blue: 249/255, alpha: 1) // Changing Color
    }
    func buttonTextColor() -> UIColor {
        return .white
    }
    func buttonBackgroundColor() -> UIColor {
       return UIColor(red: 253/255, green: 108/255, blue: 179/255, alpha: 1) // Changing Color
    }
    func textFieldBackgroundColor() -> UIColor {
        return UIColor.white.withAlphaComponent(0.4)
    }
    func appLogo() -> UIImage? {
        return UIImage(named: "CelyLogo")
    }
}

...

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
        .loginStyle: CottonCandy(), // <--- HERE!!
        .loginCompletionBlock: { ... }
    ])

    return true
}

```

#### Customize Transitions
In order to create a custom transition, create an object that conforms to [`CelyAnimator`](#Cely.CelyAnimator) protocol and set it to `.celyAnimator` inside of the [`CelyOption`](#Cely.CelyOptions) dictionary when calling Cely's [`setup(_:)`](#Cely.setup) method.

```swift
struct DefaultAnimator: CelyAnimator {
    func loginTransition(to destinationVC: UIViewController?, with celyWindow: UIWindow) {
        guard let snapshot = celyWindow.snapshotView(afterScreenUpdates: true) else {
            return
        }

        destinationVC?.view.addSubview(snapshot)
        // Set the rootViewController of the `celyWindow` object
        celyWindow.setCurrentViewController(to: destinationVC)

        // Below here is where you can create your own animations
        UIView.animate(withDuration: 0.5, animations: {
            // Slide login screen to left
            snapshot.transform = CGAffineTransform(translationX: 600.0, y: 0.0)
        }, completion: {
            (value: Bool) in
            snapshot.removeFromSuperview()
        })
    }

    func logoutTransition(to destinationVC: UIViewController?, with celyWindow: UIWindow) {
        guard let snapshot = celyWindow.snapshotView(afterScreenUpdates: true) else {
            return
        }

        destinationVC?.view.addSubview(snapshot)
        // Set the rootViewController of the `celyWindow` object
        celyWindow.setCurrentViewController(to: destinationVC)

        // Below here is where you can create your own animations
        UIView.animate(withDuration: 0.5, animations: {
            // Slide home screen to right
            snapshot.transform = CGAffineTransform(translationX: -600.0, y: 0.0)
        }, completion: {
            (value: Bool) in
            snapshot.removeFromSuperview()
        })
    }
}

...

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
    .celyAnimator: CustomAnimator(), // <--- HERE!!
    .loginCompletionBlock: { ... }
    ])

    return true
}

```


#### Use your own Screens
To use your own login screen, simply create a storyboard that contains your login screen and pass that in as `.loginStoryboard` inside of the [`CelyOptions`](#Cely.CelyOptions) dictionary when calling Cely's [`setup(_:)`](#Cely.setup) method.

Lastly, if your app uses a different storyboard other than `Main.storyboard`, you can pass that in as `.homeStoryboard`.

**⚠️⚠️⚠️⚠️ Be sure to make your Login screen as the `InitialViewController` inside of your storyboard!⚠️⚠️⚠️⚠️**

```swift

// AppDelegate.swift

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    Cely.setup(with: window!, forModel: User.ref, requiredProperties: [.token], withOptions: [
        .loginStoryboard: UIStoryboard(name: "MyCustomLogin", bundle: nil),
        .homeStoryboard: UIStoryboard(name: "NonMain", bundle: nil)
    ])

    return true
}

```

### Recommended User Pattern

```swift
import Cely

struct User: CelyUser {

    enum Property: CelyProperty {
        case username = "username"
        case email = "email"
        case token = "token"

        func securely() -> Bool {
            switch self {
            case .token:
                return true
            default:
                return false
            }
        }

        func persisted() -> Bool {
            switch self {
            case .username:
                return true
            default:
                return false
            }
        }

        func save(_ value: Any) {
            Cely.save(value, forKey: rawValue, securely: securely(), persisted: persisted())
        }

        func get() -> Any? {
            return Cely.get(key: rawValue)
        }
    }
}

// MARK: - Save/Get User Properties

extension User {

    static func save(_ value: Any, as property: Property) {
        property.save(value: value)
    }

    static func save(_ data: [Property : Any]) {
        data.forEach { property, value in
            property.save(value)
        }
    }

    static func get(_ property: Property) -> Any? {
        return property.get()
    }
}

```

The reason for this pattern is to make saving data as easy as:

```swift
// Pseudo network code, NOT REAL CODE!!!
ApiManager.logUserIn("username", "password") { json in
  let apiToken = json["token"].string

  // REAL CODE!!!
  User.save(apiToken, as: .token)
}

```
and getting data as simple as:

```swift
let token = User.get(.token)
```
