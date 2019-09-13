
---

### Writing Item

<!--
In this section: I want to go over how to use `SecItemAdd(_:)`
and what happens if you try to create something that already exists - you get a `errSecDuplicateItem`

https://developer.apple.com/documentation/security/keychain_services/keychain_items/adding_a_password_to_the_keychain
-->

Here is how to add something to Keychain.


```swift
@IBAction func AddButtonClicked(_ sender: Any) {
    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrAccount: "username",
        kSecAttrServer: "example.com",
        kSecValueData: "some-password".data(using: String.Encoding.utf8)!
    ]

    let status = SecItemAdd(query as CFDictionary, nil)
    guard status == errSecSuccess else {
        print("status:", status) // status: 0
        return
    }

    print("successfully saved")
}
```

This is a goldilock example, meaning everything is setup correctly and no error's occurred, this of course will not happen everytime. In the case you click the `AddButtonClicked(_:)` button twice, you'd expect that Keychain will add another item into its database with duplicate data, right? **Wrong!** You will get the error `-25299` which is [`errSecDuplicateItem`](https://developer.apple.com/documentation/security/errsecduplicateitem). Please read it's documentation to get a list of attributes that must be unique. So in this case, since the `kSecClass` is set to `kSecClassInternetPassword`, if `kSecAttrAccount` and `kSecAttrServer` match any other item in the keychain, regardless if we added an additional field like `kSecAttrLabel`, we will recieve the `errSecDuplicateItem` error.
```swift
let query: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecAttrAccount: account,
    kSecAttrServer: "example.com",
    kSecValueData: password
]

let status = SecItemAdd(query as CFDictionary, nil)
guard status == errSecSuccess else {
    print("status:", status) // status: 0
    return
}

// ----

let formatter = DateFormatter()
formatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
let dateStr = formatter.string(from: Date())
let query2: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecAttrAccount: account,
    kSecAttrServer: "example.com",
    kSecAttrLabel: dateStr,
    kSecValueData: password
]

let status2 = SecItemAdd(query2 as CFDictionary, nil)
guard status2 == errSecSuccess else {
    print("status:", status2) // status: -25299
    return
}
```
