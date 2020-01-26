# Cely
Cely was made to help handle user credentials and handling login with ease. Below you will find documentation for Cely's Framework. **Please do not hesitate to open an issue if something is unclear or is missing.**

## `setup(_:)`

Configuration method for Cely.

_Declaration_
```swift
static func setup(
  with window: UIWindow?,
  forModel model: CelyUser,
  requiredProperties: [CelyProperty],
  withOptions options: [CelyOptions : Any?]?
) -> Void
```

_Example_
```swift
Cely.setup(with: window, forModel: User(), requiredProperties:[.token], withOptions:[
  .homeViewController: UIHostingController(rootView: HomeContentView()),
  .loginCompletionBlock: { (username: String, password: String) in
      if username == "username" && password == "password" {
          // ...
          Cely.changeStatus(to: .loggedIn)
      }
  }
])
```

_Parameters_

Parameter | Type | Required? | Description
----|------|----------|--------
`window` | `UIWindow` | ✅ | window of your application.
`forModel` | [`CelyUser`](/api/protocols/#celyuser) | ✅ | The model Cely will be using to store data.
`requiredProperties` | `[CelyProperty]` |  | The properties that cely tests against to determine if a user is logged in. <br> **Default value**: empty array.
`options` | `[CelyOptions: Any]` |  | Dictionary of [`CelyOptions`](/api/constants/#celyoptions) to pass in additional customizations to Cely.



<br>
<br>
<br>


## `changeStatus(_:)`

Transition application between a loggedIn/loggedOut states.

_Declaration_
```swift
static func changeStatus(to status: CelyStatus)
```

_Example_
```swift
changeStatus(to: .loggedOut)
```

_Parameters_

Parameter | Type | Required? | Description
----|------|----------|--------
`status` | `CelyStatus` | ✅ | `.loggedIn` or `.loggedOut`


<br>
<br>
<br>

## `get(_:) -> Any?`

Retrieve data from store.

_Declaration_
```swift
static func get(key: String, fromStorage store: CelyStorageProtocol) -> Any?
```

_Example_
```swift
let username = Cely.get(key: "username")
```

_Parameters_

Parameter | Type| Required? | Description
----|------|----------|--------
`key` | `String` | ✅ | The key to the value you want to retrieve.
`store` | [`CelyStorageProtocol`](/api/protocols/#celystorageprotocol) | no | Storage `Cely` will be using. Defaulted to `CelyStorage`



<br>
<br>
<br>

## `save(_:) -> Result`

Saves data in store.

_Declaration_
```swift
static func save(
  _ value: Any?,
  forKey key: String,
  toStorage store: CelyStorageProtocol,
  securely secure: Bool,
  persisted persist: Bool
) -> Result<Void, Error>
```

_Example_
```swift
Cely.save("test@email.com", forKey: "email")
Cely.save("testUsername", forKey: "username", persisted: true)
Cely.save("token123", forKey: "token", securely: true)
```

_Parameters_

Parameter | Type| Required? | Description
----|------|----------|--------
`value` | `Any?` | ✅ | The value you want to save to storage.
`key` | `String` | ✅ | The key to the value you want to save.
`store` | [`CelyStorageProtocol`](/api/protocols/#celystorageprotocol) | no | Storage `Cely` will be using. Defaulted to `CelyStorage`.
`secure` | `Bool` | no | If you want to store the value securely.
`persist` | `Bool` | no | Keep data after logout.


<br>
<br>
<br>

## `currentLoginStatus() -> CelyStatus`

Returns [`CelyStatus`](/api/constants/#celystatus) of the current user.

_Declaration_
```swift
static func currentLoginStatus(
  requiredProperties properties: [CelyProperty],
  fromStorage store: CelyStorageProtocol
) -> CelyStatus
```

_Example_
```swift
let status = Cely.currentLoginStatus()
```

_Parameters_

Parameter | Type| Required? | Description
----|------|----------|--------
`properties` | [`CelyProperty`](/api/constants/#celyproperty) | no | Array of required properties that need to be in store.
`store` |  [`CelyStorageProtocol`](/api/protocols/#celystorageprotocol) | no | Storage `Cely` will be using. Defaulted to `CelyStorage`


<br>
<br>
<br>

## `logout(_:)`

Convenience method to logout user and **remove all non-persisted data.**

_Declaration_
```swift
static func logout(useStorage store: CelyStorageProtocol = store) -> Result<Void, Error>
```

_Example_
```swift
Cely.logout()
```

_Parameters_

Parameter | Type| Required? | Description
----|------|----------|--------
`store` |  [`CelyStorageProtocol`](/api/protocols/#celystorageprotocol) | no | Storage `Cely` will be using. Defaulted to `CelyStorage`

<br>
<br>
<br>

## `isLoggedIn() -> Bool`

Returns boolean whether or not the user is logged in

_Declaration_
```swift
static func isLoggedIn() -> Bool
```

_Example_
```swift
if Cely.isLoggedIn() {
  print("is logged in")
}
```
