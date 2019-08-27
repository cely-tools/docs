
Cely was made to help handle user credentials and handling login with ease. Below you will find documentation for Cely's Framework. **Please do not hesitate to open an issue if something is unclear or is missing.**

Variables
============

<div id="Cely.store"></div>

## `store`

A class that conforms to the [`CelyStorageProtocol`](#Cely.CelyStorageProtocol) protocol. By default is set to a singleton instance of `CelyStorage`.


# Methods

<div id="Cely.setup"></div>

## `setup(with:forModel:requiredProperties:withOptions:)`

Sets up Cely within your application

<summary>Example</summary>

```swift
Cely.setup(with: window, forModel: User(), requiredProperties:[.token])

// or

Cely.setup(with: window, forModel: User(), requiredProperties:[.token], withOptions:[
  .loginStoryboard: UIStoryboard(name: "MyCustomLogin", bundle: nil),
  .HomeStoryboard: UIStoryboard(name: "My_NonMain_Storyboard", bundle: nil),
  .loginCompletionBlock: { (username: String, password: String) in
        if username == "asdf" && password == "asdf" {
            print("username: \(username): password: \(password)")
        }
    }
])
```


<summary>Parameters</summary>

Key | Type| Required? | Description
----|------|----------|--------
`window` | `UIWindow` | ✅ | window of your application.
`forModel` | [`CelyUser`](#Cely.CelyUser) | ✅ | The model Cely will be using to store data.
`requiredProperties` | `[CelyProperty]` | no | The properties that cely tests against to determine if a user is logged in. <br> **Default value**: empty array.
`options` | `[CelyOption]` | no | An array of [`CelyOptions`](#Cely.CelyOptions) to pass in additional customizations to cely.




<div id="Cely.currentLoginStatus"></div>

## `currentLoginStatus(requiredProperties:fromStorage:)`

Will return the `CelyStatus` of the current user.

<summary>Example</summary>

```swift
let status = Cely.currentLoginStatus()
```



<summary>Parameters</summary>

Key | Type| Required? | Description
----|------|----------|--------
`properties` | [`CelyProperty`] | no | Array of required properties that need to be in store.
`store` | `CelyStorage` | no |    Storage `Cely` will be using. Defaulted to `CelyStorage`




<summary>Returns</summary>

Type| Description
----|------
[`CelyStatus`](#Cely.CelyStatus) | If `requiredProperties` are all in store, it will return `.loggedIn`, else `.loggedOut`




<div id="Cely.get"></div>

## `get(_:fromStorage:)`

Returns stored data for key.

<summary>Example</summary>

```swift
let username = Cely.get(key: "username")
```


<summary>Parameters</summary>

Key | Type| Required? | Description
----|------|----------|--------
`key` | String | ✅ | The key to the value you want to retrieve.
`store` | CelyStorage | no | Storage `Cely` will be using. Defaulted to `CelyStorage`




<summary>Returns</summary>

Type| Description
----|------
`Any?` | Returns an `Any?` object in the case the value nil(not found).






<div id="Cely.save"></div>

## `save(_:forKey:toStorage:securely:persisted:)`

Saves data in store

<summary>Example</summary>

```swift
Cely.save("test@email.com", forKey: "email")
Cely.save("testUsername", forKey: "username", persisted: true)
Cely.save("token123", forKey: "token", securely: true)
```


<summary>Parameters</summary>

Key | Type| Required? | Description
----|------|----------|--------
`value` | <code>**Any?**</code> | ✅ | The value you want to save to storage.
`key` | <code>**String**</code> | ✅ | The key to the value you want to save.
`store` | <code>**CelyStorage**</code> | no | Storage `Cely` will be using. Defaulted to `CelyStorage`.
`secure` | <code>**Boolean**</code> | no | If you want to store the value securely.
`persisted` | <code>**Boolean**</code> | no | Keep data after logout.




<summary>Returns</summary>

Type| Description
----|------
  `StorageResult` | Whether or not your value was successfully set.




<div id="Cely.changeStatus"></div>

## `changeStatus(to:)`

Perform action like `LoggedIn` or `LoggedOut`.

<summary>Example</summary>

```swift
changeStatus(to: .loggedOut)
```


<summary>Parameters</summary>

Key | Type| Required? | Description
----|------|----------|--------
`status` | <code>**CelyStatus**</code> | ✅ | enum value




<div id="Cely.logout"></div>

## `logout(usesStorage:)`

Convenience method to logout user. Is equivalent to `changeStatus(to: .loggedOut)`

<summary>Example</summary>


```swift
Cely.logout()
```


<summary>Parameters</summary>

Key | Type| Required? | Description
----|------|----------|--------
`store` | <code>**CelyStorage**</code> | no | Storage `Cely` will be using. Defaulted to `CelyStorage`.




<div id="Cely.isLoggedIn"></div>

## `isLoggedIn()`

Returns whether or not the user is logged in

<summary>Example</summary>

```swift
Cely.isLoggedIn()
```


<summary>Returns</summary>

Type| Description
----|------
`Boolean` | Returns whether or not the user is logged in




 Constants
# Protocols
<div id="Cely.CelyUser"></div>

## `CelyUser `

`protocol` for model class to implements

<summary>Example</summary>

```swift
import Cely

struct User: CelyUser {

  enum Property: CelyProperty {
    case token = "token"
  }
}
```



<summary>Required</summary>

value | Type| Description
----|------|---
`Property ` | `associatedtype` | Enum of all the properties you would like to save for a model



<div id="Cely.CelyStorageProtocol"></div>

## `CelyStorageProtocol `

`protocol` a storage class must abide by in order for Cely to use it


<summary>Required</summary>

```swift
func set(_ value: Any?, forKey key: String, securely secure: Bool, persisted: Bool) -> StorageResult
func get(_ key: String) -> Any?
func removeAllData()

```


<div id="Cely.CelyStyle"></div>


## `CelyStyle`

The `protocol` an object must conform to in order to customize Cely's default login screen. Since all methods are optional, Cely will use the default value for any unimplemented methods.


<summary>Methods</summary>

```swift
func backgroundColor() -> UIColor
func textFieldBackgroundColor() -> UIColor
func buttonBackgroundColor() -> UIColor
func buttonTextColor() -> UIColor
func appLogo() -> UIImage?

```


<div id="Cely.CelyAnimator"></div>


## `CelyAnimator`

The `protocol` an object must conform to in order to customize transitions between home and login screens.


<summary>Methods</summary>

```swift
func loginTransition(to destinationVC: UIViewController?, with celyWindow: UIWindow)
func logoutTransition(to destinationVC: UIViewController?, with celyWindow: UIWindow)

```


# Typealias
<div id="Cely.CelyProperty"></div>

## `CelyProperty `

`String` type alias. Is used in User model

<div id="Cely.CelyCommands"></div>

## `CelyCommands `

`String` type alias. Command for cely to execute

# enums
<div id="Cely.CelyOptions"></div>

## `CelyOptions`

`enum` Options that you can pass into Cely on [`setup(with:forModel:requiredProperties:withOptions:)`](#Cely.setup)


<summary>Options</summary>

Case | Description
----|------
`storage ` | Pass in you're own storage class if you wish not to use Cely's default storage. Class must conform to the `CelyStorage` protocol.
`homeStoryboard ` | Pass in your app's default storyboard if it is not named "Main"
`loginStoryboard ` | Pass in your own login storyboard.
`loginStyle` | Pass in an object that conforms to [`CelyStyle`](#Cely.CelyStyle) to customize the default login screen.
`loginCompletionBlock ` | `(String,String) -> Void` block of code that will run once the Login button is pressed on Cely's default login Controller
`celyAnimator` | Pass in an object that conforms to [`CelyAnimator`](#Cely.CelyAnimator) to customize the default login screen.


<div id="Cely.CelyStatus"></div>

## `CelyStatus`

`enum` Statuses for Cely to perform actions on


<summary>Statuses</summary>

Case | Description
----|------
`loggedIn ` | Indicates user is now logged in.
`loggedOut ` | Indicates user is now logged out.



<div id="Cely.StorageResult"></div>

## `StorageResult `

`enum` result on whether or not Cely successfully saved your data.


<summary>Results</summary>

Case| Description
----|------
`success ` | Successfully saved your data
`fail(error) ` | Failed to save data along with a `LocksmithError`.


