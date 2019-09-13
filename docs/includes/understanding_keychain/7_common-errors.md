
---


## Common Errors

<!--
In this section:
I want to go over common errors that you might encounter when working with keychain

errSecParam: -50
errSecItemNotFound: -25300
errSecDuplicateItem: -25299
errSecNoSuchAttr: -25303
https://developer.apple.com/documentation/security/errsecduplicateitem
-->

### errSecItemNotFound

`errSecItemNotFound`, error `-25300`, occurs when performing an Keychain operation such as [`SecItemCopyMatching(_:)`](https://developer.apple.com/documentation/security/1398306-secitemcopymatching), [`SecItemUpdate(_:)`](https://developer.apple.com/documentation/security/1393617-secitemupdate), or [`SecItemDelete(_:)`](https://developer.apple.com/documentation/security/1395547-secitemdelete) and Keychain fails to find any matching items. In the case of `SecItemUpdate(_:)` and `SecItemDelete(_:)`, its best to query for the item before trying to perform an operation.

### errSecParam

`errSecParam`, error `-50`, occurs when you fail to properly set an attribute. Where I see this happening the most is when you fail to properly encode the `kSecValueData` as `CFData` or you fail to provide a necessary attribute such as `kSecClass`. I would investigate those two attributes first when you encounter `errSecParam`.

### errSecNoSuchAttr

`errSecNoSuchAttr`, error `-25303`, occurs when you try to set an attribute that doesn't exist for the provided `kSecClass`. In the bottom example we try to set `kSecAttrIssuer`, which is only available for the `kSecClassCertificate` class.

```swift
let password = "some-password".data(using: String.Encoding.utf8)!
let query: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecAttrAccount: "username",
    kSecAttrIssuer: "Fabian issuer".data(using: String.Encoding.utf8)!, // kSecClassCertificate only
    kSecAttrServer: "example.com",
    kSecValueData: password
]

let status = SecItemAdd(query as CFDictionary, nil)
guard status == errSecSuccess else {
    print("status:", status) // status: -25303
    return
}
```
