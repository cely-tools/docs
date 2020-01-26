
---

## Add FaceID/TouchID

### Access Control

<!--
In this section:
- NSFaceIDUsageDescription
- I want to explain what `kSecAttrAccessControl` is and why is it important.
- relationship between `kSecAttrAccessible` and `kSecAttrAccessControl`
  - what happens if you set conflicting accessible attributes (`kSecAttrAccessControl` & `kSecAttrAccessible`)?!
    - Result: you get a -50 error
-->

In order to add biometrics such as FaceID or TouchID to a Keychain item, you must create a [`SecAccessControl`](https://developer.apple.com/documentation/security/secaccesscontrol) object using the [`SecAccessControlCreateWithFlags(_:)`](https://developer.apple.com/documentation/security/1394452-secaccesscontrolcreatewithflags) function and set it the [`kSecAttrAccessControl`](https://developer.apple.com/documentation/security/ksecattraccesscontrol) attribute.

_example_

```swift
var error: Unmanaged<CFError>? = nil
let accessControlObject = SecAccessControlCreateWithFlags(nil,
                                            kSecAttrAccessibleWhenUnlocked,
                                            .userPresence,
                                            &error)

let item: [CFString: Any] = [
    kSecClass: kSecClassInternetPassword,
    kSecAttrAccessControl: accessControlObject
    // ...
]

// ...
```

### Flags

<!--
In this section:
I want to go over the different flags and what type of errors to expect in certain scenarios
-->

In this section we will be going over some of the flags you can set and what errors you may encounter. In you want to see the entire list of flags, please checkout [`SecAccessControlCreateFlag`](https://developer.apple.com/documentation/security/secaccesscontrolcreateflags) for more information.

Flags we will go over:

- `.userPresence` - FaceID/TouchID/Device Passcode.
- `.devicePasscode` - Device Passcode only.
- `.biometryAny` - FaceID/TouchID only (allows for biometrics changes).
- `.biometryCurrentSet` - FaceID/TouchID only (invalidates item when biometrics change).
- `.applicationPassword` - Application Specific password.

#### NSFaceIDUsageDescription

When using `.userPresence`, if [`NSFaceIDUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nsfaceidusagedescription) is not set in your plist, your application will revert to prompting the user for the Device Passcode. But when using `.biometryAny` or `.biometryCurrentSet`, failing to set `NSFaceIDUsageDescription` will result in your application crashing with the following error:

```text
ERROR: This app has crashed because it attempted to access privacy-sensitive
data without a usage description. The app's Info.plist must contain
an NSFaceIDUsageDescription key with a string value explaining to
the user how the app uses this data.
```

#### .userPresence

[`.userPresence`](https://developer.apple.com/documentation/security/secaccesscontrolcreateflags/1392879-userpresence) is more than likely the flag you would be using in your application. Other flags such as `.devicePasscode` and `.biometryAny`, are simply slight derivatives of this flag.

![](../../images/understanding-keychain/userPresence-flow.jpg)

Above is the flow you can expect for your application to take when saving/retrieving a Keychain Item with the `.userPresence` flag. **It is very possible to encounter [`errSecAuthFailed: -25293`](./#errsecauthfailed) errors when working with `.userPresence` flagged items.** Please review the above flows for a better understanding. In addition to the _Retrieving Flow_, If the user adds back a passcode to the device *(it doesn't have to be the same passcode)* the item can once again be retrieved.


#### .biometryCurrentSet

The flag that differs from `.userPresence` the most would be [.biometryCurrentSet](https://developer.apple.com/documentation/security/secaccesscontrolcreateflags/2937192-biometrycurrentset), it invalidates any Keychain Items once a finger is added/removed on TouchID or if the user re-enrolls for FaceID.

![](../../images/understanding-keychain/biometryCurrentSet-flow.jpg)

#### .applicationPassword

The last flag we will go over is [.applicationPassword](https://developer.apple.com/documentation/security/secaccesscontrolcreateflags/1617930-applicationpassword), this allows for the user to set an application specific passcode in order to retrieve the item. When using this flag, iOS will prompt the user to insert a passcode. Meaning, you don't need to worry about providing a view to capture the application password. Unlike retrieving an item using device security such as `.devicePasscode`, after 5 failed attempts, Keychain returns the error `errSecAuthFailed: -25293` instead of disabling the item retrieval. Meaning, after 5 failed attempts the user can simply try again without being penalized.


#### Combining Flags

In the case you want to authenticate the user with both `.userPresence` and `.applicationPassword`, you may combine them using an array.

```swift
var error: Unmanaged<CFError>? = nil
let access = SecAccessControlCreateWithFlags(nil,
                                            kSecAttrAccessibleWhenUnlocked,
                                            [.userPresence, .applicationPassword],
                                            &error)
```

Some combinations are not allowed such as `[.userPresence, .devicePasscode]`, so be sure to print out any errors for more information.

### FaceID/TouchID Example

In this example, we will create an item that will require FaceID, TouchID, or Passcode in order to be retrieved.

```swift
@IBAction func saveWithAccessFlagClicked(_ sender: Any) {
    var error: Unmanaged<CFError>? = nil
    guard let access = SecAccessControlCreateWithFlags(nil,
                                                      kSecAttrAccessibleWhenUnlocked,
                                                      .userPresence,
                                                      &error) else { return }

    let item: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrAccessControl: access,
        kSecAttrServer: "example.com",
        kSecAttrAccount: "username",
        kSecValueData: "some-password".data(using: String.Encoding.utf8)!
    ]

    let status = SecItemAdd(item as CFDictionary, nil)
    guard status == errSecSuccess else {
        print("status:", status)
        return
    }

    print("successfully saved with access attribute")
}
```

Next, we will retrieve the `.userPresence` flagged item.

```swift
@IBAction func QueryClicked(_ sender: Any) {

    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecAttrServer: "example.com",
        kSecAttrAccount: "username",
        kSecReturnAttributes: true,
        kSecReturnData: true
    ]

    var someItem: CFTypeRef?
    let status = SecItemCopyMatching(query as CFDictionary, &someItem)
    guard status == errSecSuccess else {
        print("status:", status)
        return
    }

    guard let item = someItem,
        let username = item[kSecAttrAccount] as? String,
        let passwordData = item[kSecValueData] as? Data,
        let password = String(data: passwordData, encoding: .utf8) else {
        print("Item not found")
        return
    }

    print("username:", username)
    print("password:", password)
}
```

### kSecUseAuthenticationUISkip

<!--
In this section:
- Querying Keychain and returns 2 items, one of which is protected by faceID. - What happens?
- go over:
  - [kSecUseAuthenticationUISkip](https://developer.apple.com/documentation/security/ksecuseauthenticationuiskip) - Silently skip any items that require user authentication.
-->

When querying items from Keychain, you may be returned items that have the `kSecAttrAccessControl` attribute set, which will require the user to authenticate in order to retrieve the item. You can exclude these results by setting the [`kSecUseAuthenticationUI`](https://developer.apple.com/documentation/security/ksecuseauthenticationui) attribute in your query.


```swift

@IBAction func QueryAllClicked(_ sender: Any) {

    let query: [CFString: Any] = [
        kSecClass: kSecClassInternetPassword,
        kSecMatchLimit: kSecMatchLimitAll,
        kSecUseAuthenticationUI: kSecUseAuthenticationUISkip,
        ...
    ]

    var someItem: CFTypeRef?
    let status = SecItemCopyMatching(query as CFDictionary, &someItem)
    ...
```

#### Notes

When setting a `kSecAttrAccessControl` attribute on your Keychain Item, you cannot set the `kSecAttrAccessible` attribute on the Keychain Item itself. Doing so will result in a `errSecParam: -50` error, even if both are set to the same value. As an example, the following will return an error:

```swift
var error: Unmanaged<CFError>? = nil
guard let access = SecAccessControlCreateWithFlags(nil,
                                                  kSecAttrAccessibleWhenUnlocked, // <--
                                                  .userPresence,
                                                  &error) else { return }

let query: [CFString: Any] = [
    kSecAttrAccessControl: access,
    kSecAttrAccessible:kSecAttrAccessibleWhenUnlocked, // <--
    //...
]

let status = SecItemAdd(query as CFDictionary, nil) // equals -50 (errSecParam)
```




