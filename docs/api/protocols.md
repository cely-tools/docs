## `CelyUser `

<br>

`protocol` for an object to implement to allow Cely have access to the properties associated to a user.

```swift
protocol CelyUser {
    /// Enum of all the properties you would like to save for a model
    associatedtype Property : RawRepresentable
}
```

```swift
// Usage
import Cely

struct User: CelyUser {

  enum Property: CelyProperty {
    case token = "token"
  }
}
```


<br>
<br>
<br>



## `CelyStorageProtocol `

<br>

In the case you need more control of how user information is stored, you can set the `.storage` Cely option in [`Cely.setup(...)`](../#setup) to an object that conforms to `CelyStorageProtocol`.


```swift
/// Protocol a storage class must abide by in order for Cely to use it
public protocol CelyStorageProtocol {
    func set(_ value: Any?, forKey key: String, securely secure: Bool, persisted persist: Bool) -> Result<Void, CelyStorageError>
    func get(_ key: String) -> Any?
    func removeAllData()
}
```





<br>
<br>
<br>

## `CelyStyle`

<br>

The `protocol` an object must conform to in order to customize Cely's default login screen. Since all methods are optional, Cely will use the default value for any unimplemented methods.


```swift
/// Protocol that allows styles to be applied to Cely's default LoginViewController
public protocol CelyStyle {
    func backgroundColor() -> UIColor
    func textFieldBackgroundColor() -> UIColor
    func buttonBackgroundColor() -> UIColor
    func buttonTextColor() -> UIColor
    func appLogo() -> UIImage?
}
```




<br>
<br>
<br>

## `CelyAnimator`

<br>

The `protocol` an object must conform to in order to customize transitions between home and login screens.


```swift
/// Handles Animations between Home and Login ViewControllers
public protocol CelyAnimator {
    func loginTransition(to destinationVC: UIViewController?, with celyWindow: UIWindow)
    func logoutTransition(to destinationVC: UIViewController?, with celyWindow: UIWindow)
}
```

