# Cely Credentials

### `set(_:) -> Result`

Securely store user credentials.

_Declaration_
```swift
public func set(
  username: String,
  password: String,
  server: String,
  accessibility: [AccessibilityOptions]
) -> Result<Void, Error>
```

_Example_
```swift
// Example
let credentialResult = Cely.credentials.set(
    username: username,
    password: password,
    server: "api.example.com"
    accessibility: [.biometricsIfPossible]
)

switch credentialResult {
case let .success:
    print("success!")
case let .failure(error):
    print("failed to store credentials")
}
```

_Parameters_

Parameter | Type | Required? | Description
----|------|----------|--------
`username` | `String` | ✅ | username for user.
`password` | `String` | ✅ | password for user.
`server` | `String` | ✅ | API uri for account.
`accessibility` | [`[AccessibilityOptions]`](/api/constants/#accessibilityoptions) | no | Array of `AccessibilityOptions` for credentials to be saved with.


<br>
<br>
<br>

### `get() -> Result`

Retrieve the current user's credentials

_Declaration_
```swift
func get() -> Result<CelyCredentials, Error>
```

_Example_
```swift
let result = Cely.credentials.get()

switch result {
case let .success(credentials):
    print(credentials)
case let .failure(error):
    print("failed to get credentials")
}
```
