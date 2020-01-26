
---

## Keychain Anatomy

> **Note**
>
> Keychain Services expects Core Foundation objects, hence the `CF` prefix on some of the properties/attributes, such as `CFDictionary`, `CFData`, etc. Don't be intimidated, you can still use the `Data` type since Apple has bridges for these types. Read [Tool-Free Bridging](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Toll-FreeBridgin/Toll-FreeBridgin.html) for more information.


The Keychain is an encrypted database stored on disk consistenting of [Keychain Items](https://developer.apple.com/documentation/security/keychain_services/keychain_items). A Keychain Item is made up of attributes and the data you wish to encrypt.

### kSecClass

The `kSecClass` attribute describes to Keychain what [classification](https://developer.apple.com/documentation/security/keychain_services/keychain_items/item_class_keys_and_values) an item is, and determines what other attributes can be stored with your item.

For example, if your application was storing a certificate, you would use the `kSecClassCertificate` class attribute. This attribute tells Keychain what other attributes can be set on your item, such as `kSecAttrIssuer` and `kSecAttrPublicKeyHash`. Trying to set an attribute that doesn't exist on the `kSecClass` attribute will result in `OSStatus` code `errSecNoSuchAttr: -25303`. Also, not setting the `kSecClass` key in your Keychain Item will result in `OSStatus` error `errSecParam: -50`.

Below are the available options for the `kSecClass` key:

- [kSecClassGenericPassword](https://developer.apple.com/documentation/security/ksecclassgenericpassword)
- [kSecClassInternetPassword](https://developer.apple.com/documentation/security/ksecclassinternetpassword)
- [kSecClassCertificate](https://developer.apple.com/documentation/security/ksecclasscertificate)
- [kSecClassKey](https://developer.apple.com/documentation/security/ksecclasskey)
- [kSecClassIdentity](https://developer.apple.com/documentation/security/ksecclassidentity)

If you want to see all available attributes regardless of class, here is the [list](https://developer.apple.com/documentation/security/keychain_services/keychain_items/item_attribute_keys_and_values).
