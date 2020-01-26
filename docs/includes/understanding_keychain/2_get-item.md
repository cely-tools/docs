
---

### Querying Item(s)

<!--
In this section:
- I want to go over how to use `SecItemCopyMatching(_:)`
- explain `kSecReturnData: true`
- When you save duplicate items you get `errSecDuplicateItem`

https://developer.apple.com/documentation/security/keychain_services/keychain_items/searching_for_keychain_items
-->

Note from [Apple documentation](https://developer.apple.com/documentation/security/keychain_services/keychain_items/searching_for_keychain_items#2922072):

> By default, your app can freely retrieve its own keychain items but not those of other apps. However, keychain services does provide mechanisms for broadening or narrowing that accessibility, for example, using the [`kSecAttrAccessGroup`](https://developer.apple.com/documentation/security/ksecattraccessgroup) attribute.

Use the [`SecItemCopyMatching(_:)`](https://developer.apple.com/documentation/security/1398306-secitemcopymatching) function to query item(s) from Keychain. With the example below, take note that we are only searching across `kSecAttrServer` and not including `kSecAttrAccount`.

```swift
@IBAction func QueryClicked(_ sender: Any) {

    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrServer: "example.com",
        kSecReturnAttributes: true,
        kSecReturnData: true
    ]

    var someItem: CFTypeRef?
    let status = SecItemCopyMatching(query as CFDictionary, &someItem)
    guard status == errSecSuccess else {
        print("status:", status)
        return
    }

    guard let item = someItem else {
        print("no Item") // errSecItemNotFound: -25300
        return
    }

    print("result: \(item)")
}
```

In the case where multiple accounts have the same `kSecAttrServer` set, we'd expect to see an array of items returned. But with the above example, only one item was returned. This is because [**by default `SecItemCopyMatching(_:)` only returns the first match found**](https://developer.apple.com/documentation/security/1398306-secitemcopymatching#discussion). If you would like to return all items that match the provided query, you must set `kSecMatchLimit` in the query.

```swift
let query: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecMatchLimit: kSecMatchLimitAll, // <--
    // ...
]
```

The two most important attributes when retrieving an item are:

- [`kSecReturnAttributes`](https://developer.apple.com/documentation/security/ksecreturnattributes): **must be set to `true` in order to retrieve item's attributes**
- [`kSecReturnData`](https://developer.apple.com/documentation/security/ksecreturndata): must be set to true in order to retrieve secret data.


Even setting `kSecReturnData: true` and excluding `kSecReturnAttributes`, or setting it to false, will result in `item` returning as `nil`.

```swift
let query: [CFString: Any] = [
    //...
    kSecReturnAttributes: false,
    kSecReturnData: true
]

// ...

// prints: no Item
```

