
## `CelyOptions`

Parameter | Type | Description
----|------|----
`.homeViewController ` | `UIViewController` | App's Home `ViewController`.
`.loginViewController ` | `UIViewController` | App's Login `ViewController`.
`.celyAnimator` | [`CelyAnimator`](/api/protocols/#celyanimator) | Custom animation when login/logout occurs.
`.loginStyle` | [`CelyStyle`](/api/protocols/#celystyle)  | Customize Cely's default `LoginViewController`.
`.loginCompletionBlock ` | `(username: String, password: String) -> Void` | Handle **Login** click on Cely's default `LoginViewController`.
`.storage ` | [`CelyStorageProtocol`](/api/protocols/#celystorageprotocol) | Override Cely's storage with custom class.


<br>
<br>
<br>

---

## `AccessibilityOptions`

Parameter | Description
----|------
`.biometricsIfPossible` | Store/retrieve credentials with biometrics (FaceID/TouchID).
`.thisDeviceOnly` | Only allow for credentials to be stored on current device.


<br>
<br>
<br>

---

## `CelyStatus`

`enum` Statuses for Cely to perform actions on


Parameter | Description
----|------
`.loggedIn ` | Indicates user is now logged in.
`.loggedOut ` | Indicates user is now logged out.



<br>
<br>
<br>


## `CelyStorageError`

`CelyStorageError` is an `enum` built to handle the possible errors that may come from working with Keychain Services. `CelyStorageError` was modeled after KeychainAccess's [`Status` enum](https://github.com/kishikawakatsumi/KeychainAccess/blob/3a9c83cf8b8cfaecd1097916fae803e1b1d6447f/Lib/KeychainAccess/Keychain.swift#L1695).


<br>
<br>
<br>

---

### Typealiases


## `CelyProperty `


`String` typealias. Is used in User model


## `CelyCommands `


`String` typealias. Command for Cely to execute
