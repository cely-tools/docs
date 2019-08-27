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
