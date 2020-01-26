
---

### Create Item

<!--
In this section: I want to go over how to use `SecItemAdd(_:)`
and what happens if you try to create something that already exists - you get a `errSecDuplicateItem`

https://developer.apple.com/documentation/security/keychain_services/keychain_items/adding_a_password_to_the_keychain
-->

Use the [`SecItemAdd(_:)`](https://developer.apple.com/documentation/security/1401659-secitemadd) function to add an item to Keychain. Below is an example on how to add an item to Keychain.

```swift
@IBAction func AddButtonClicked(_ sender: Any) {
    let item: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrAccount: "username",
        kSecAttrServer: "example.com",
        kSecValueData: "some-password".data(using: String.Encoding.utf8)!
    ]

    let status = SecItemAdd(item as CFDictionary, nil)
    guard status == errSecSuccess else {
        print("status:", status) // status: 0
        return
    }

    print("successfully saved")
}
```

This is a goldilock example, meaning everything is setup correctly and no error's occurred, this of course will not happen everytime. In the case `AddButtonClicked(_:)` executes twice, you'd expect that Keychain will add another item into its database with duplicate data, right? **Wrong!** This will result in error `errSecDuplicateItem: -25299`. Please read [`errSecDuplicateItem` documentation](https://developer.apple.com/documentation/security/errsecduplicateitem) to get a set of attributes (primary keys) that must be unique.

In the following example, we are trying to store two `kSecClassInternetPassword` items in the Keychain. Since the set of `kSecAttrAccount` and `kSecAttrServer` are not unique between the items, the `item2` will result in the `errSecDuplicateItem` error.

```swift
let item1: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecAttrAccount: account,
    kSecAttrServer: "example.com",
    kSecValueData: passwordData
]

let status = SecItemAdd(item1 as CFDictionary, nil)
guard status == errSecSuccess else {
    print("status:", status) // status: 0
    return
}

// ----

let item2: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecAttrAccount: account,
    kSecAttrServer: "example.com",
    kSecValueData: passwordData,
    kSecAttrLabel: "some new label attribute"
]

let status2 = SecItemAdd(item2 as CFDictionary, nil)
guard status2 == errSecSuccess else {
    print("status:", status2) // status: -25299
    return
}
```
