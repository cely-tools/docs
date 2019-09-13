
---

## Accessibility

<!--
In this section:
I want to talk about the different Accessible properties
-->

In this section we will go over how to configure [***when*** your application has access to your Keychain items](https://developer.apple.com/documentation/security/keychain_services/keychain_items/restricting_keychain_item_accessibility). For this, we set the [`kSecAttrAccessible`](https://developer.apple.com/documentation/security/ksecattraccessible) attribute.

_Example_
```swift
let query: [CFString: Any] = [
  kSecAttrAccessible: kSecAttrAccessibleWhenUnlocked,
  // ...
]
```

By default, [this attribute it set to `kSecAttrAccessibleWhenUnlocked`](https://developer.apple.com/documentation/security/keychain_services/keychain_items/restricting_keychain_item_accessibility#2974972), but values for this attribute fall within two categories:

Items that <u>**can**</u> be restored from a backup of another device.

- [kSecAttrAccessibleWhenUnlocked](https://developer.apple.com/documentation/security/ksecattraccessiblewhenunlocked)
- [kSecAttrAccessibleAfterFirstUnlock](https://developer.apple.com/documentation/security/ksecattraccessibleafterfirstunlock)

Items that <u>**cannot**</u> be restored from a backup of another device.

- [kSecAttrAccessibleWhenUnlockedThisDeviceOnly](https://developer.apple.com/documentation/security/ksecattraccessiblewhenunlockedthisdeviceonly)
- [kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly](https://developer.apple.com/documentation/security/ksecattraccessibleafterfirstunlockthisdeviceonly)
- [kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly](https://developer.apple.com/documentation/security/ksecattraccessiblewhenpasscodesetthisdeviceonly)

If you are using `kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly` – be prepared to encounter the [`errSecNotAvailable`](https://www.osstatus.com/search/results?platform=all&framework=Security&search=25291) error if the user does not have a passcode set on their device. Here is where you can prompt the user that: "They must set a device passcode in order to continue."

### Notes

You must also provide the item's encrypted data, `kSecValueData`, anytime you wish to update the `kSecAttrAccessible` attribute in the future.
