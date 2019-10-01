
## `enum CelyOptions`

Case | Type | Description
----|------|----
`.homeViewController ` | `UIViewController` | App's Home `ViewController`.
`.loginViewController ` | `UIViewController` | App's Login `ViewController`.
`.celyAnimator` | [`CelyAnimator`](./#celyanimator) | Custom animation when login/logout occurs.
`.loginStyle` | [`CelyStyle`](./#celystyle)  | Customize Cely's default `LoginViewController`.
`.loginCompletionBlock ` | `(username: String, password: String) -> Void` | Handle **Login** click on Cely's default `LoginViewController`.
`.storage ` | [`CelyStorage`](./#celystorage) | Override Cely's storage with custom class.


<br>
<br>
<br>

---

## `CelyStatus`

`enum` Statuses for Cely to perform actions on


Case | Description
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
