
---

### Deleting Item

<!--
In this section: I want to go over how to use `SecItemDelete(_:)`
and what happens if you try to delete something that doesnt exist

https://developer.apple.com/documentation/security/keychain_services/keychain_items/updating_and_deleting_keychain_items
-->

Below is an example on how to delete an item in our Keychain. Encountering the [`errSecItemNotFound`](./#errsecitemnotfound) error is common with this operation.

```swift
@IBAction func deleteClicked(_ sender: Any) {
    let account = "username"
    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrAccount: account,
        kSecAttrServer: "example.com"
    ]

    let status = SecItemDelete(query as CFDictionary)
    guard status == errSecSuccess else {
        print("status:", status)
        return
    }

    print("successfully deleted")
}
```

It is important to note that you should provide as many attributes to help Keychain filter what item to delete. In the case where a user has multiple accounts stored in Keychain and you simply queried for `kSecAttrServer` and `kSecClass`, all Keychain Items that match that query will be deleted.
