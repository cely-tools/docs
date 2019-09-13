
---

### Updating Item

<!--
In this section: I want to go over how to use `SecItemUpdate(_:)`
and what happens if you try to update something that doesnt exist

https://developer.apple.com/documentation/security/keychain_services/keychain_items/updating_and_deleting_keychain_items
-->

Here we're going to update an item in our Keychain. Encountering the [`errSecItemNotFound`](./#errsecitemnotfound) error is common with this operation.

```swift
@IBAction func updateClicked(_ sender: Any) {

    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrServer: "example.com",
        kSecAttrAccount: "username"
    ]

    let attributes: [CFString: Any] = [
        kSecValueData: "new-password".data(using: String.Encoding.utf8)!
    ]
    let status = SecItemUpdate(query as CFDictionary, attributes as CFDictionary)

    guard status == errSecSuccess else {
        print("status:", status)
        return
    }

    print("successfully updated")
}
```

It is important to note that you should provide as many attributes to help Keychain filter what item to update. In the case where a user has multiple accounts stored in Keychain and you simply queried for `kSecAttrServer` and `kSecClass`, all Keychain Items that match that query will be updated.

