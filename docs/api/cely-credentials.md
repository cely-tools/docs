# Cely Credentials

### `set(_:) -> Result`

Securely store user credentials.

_Declaration_
```swift
public func set(
  username: String,
  password: String,
  server: String,
  controlOptions: [AccessControlOptions]
) -> Result<Void, Error>
```

_Example_
```swift
// Example
let credentialResult = Cely.credentials.set(
    username: username,
    password: password,
    server: "api.example.com"
    controlOptions: [.biometricsIfPossible]
)

switch credentialResult {
case let .success:
    print("success!")
case let .failure(error):
    print("failed to store credentials")
}
```

_Parameters_

| Parameter        | Type                                                             | Required? | Description                                                       |
| ---------------- | ---------------------------------------------------------------- | --------- | ----------------------------------------------------------------- |
| `username`       | `String`                                                         | ✅         | username for user.                                                |
| `password`       | `String`                                                         | ✅         | password for user.                                                |
| `server`         | `String`                                                         | ✅         | API uri for account.                                              |
| `controlOptions` | [`[AccessControlOptions]`](/api/constants/#accesscontroloptions) | no        | Array of `AccessControlOptions` for credentials to be saved with. |


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
