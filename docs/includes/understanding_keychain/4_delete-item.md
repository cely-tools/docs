
---

### Deleting Item

<!--
In this section: I want to go over how to use `SecItemDelete(_:)`
and what happens if you try to delete something that doesnt exist

https://developer.apple.com/documentation/security/keychain_services/keychain_items/updating_and_deleting_keychain_items
-->

Use the [`SecItemDelete(_:)`](https://developer.apple.com/documentation/security/1395547-secitemdelete) function to delete an item in Keychain. Just like when [updating an item](./#updating-item), you should provide as many attributes to help Keychain filter what item to delete. In the case where a user has multiple accounts stored in Keychain and you simply queried for `kSecAttrServer` and `kSecClass`, all Keychain Items that match that query will be deleted.

```swift
@IBAction func deleteClicked(_ sender: Any) {
    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrAccount: "username",
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
