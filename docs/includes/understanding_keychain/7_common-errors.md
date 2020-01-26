
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

### errSecItemNotFound: -25300

[`errSecItemNotFound`](https://developer.apple.com/documentation/security/errsecitemnotfound), occurs when performing an Keychain operation such as [`SecItemCopyMatching(_:)`](https://developer.apple.com/documentation/security/1398306-secitemcopymatching), [`SecItemUpdate(_:)`](https://developer.apple.com/documentation/security/1393617-secitemupdate), or [`SecItemDelete(_:)`](https://developer.apple.com/documentation/security/1395547-secitemdelete) and Keychain fails to find any matching items. In the case of `SecItemUpdate(_:)` and `SecItemDelete(_:)`, its best to query for the item before trying to perform an operation.

### errSecParam: -50

[`errSecParam`](https://developer.apple.com/documentation/security/errSecParam), occurs when you fail to properly set an attribute. Where I see this happening the most is when you fail to properly encode the `kSecValueData` as `CFData` or you fail to provide a necessary attribute such as `kSecClass`. I would investigate those two attributes first when you encounter `errSecParam`.

### errSecAuthFailed: -25293

[`errSecAuthFailed`](https://developer.apple.com/documentation/security/errsecauthfailed), occurs when your application is trying to save an item that requires access control, but FaceID, TouchID, or Device Passcode are not setup on the device. Or if the user has removed the device's security since saving the item.

### errSecNoSuchAttr: -25303

[`errSecNoSuchAttr`](https://developer.apple.com/documentation/security/errSecNoSuchAttr), occurs when you try to set an attribute that doesn't exist for the provided `kSecClass`. In the bottom example we try to set `kSecAttrIssuer`, which is only available for the `kSecClassCertificate` class.

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
