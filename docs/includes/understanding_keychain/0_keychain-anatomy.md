
---

## Keychain Anatomy

<!--
Diagram of: Keychain database full of Keychain Items
-->

> **Note**
>
> Keychain Services expects Core Foundation objects, hence the `CF` prefix on some of the properties/attributes it expects, such as `CFDictionary`, `CFData`, etc. Don't be intimidated, you can still use the `Data` type since Apple has bridges for these types. Read [Tool-Free Bridging](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Toll-FreeBridgin/Toll-FreeBridgin.html) for more information.


The Keychain is an encrypted database stored on disk consistenting of [Keychain Items](https://developer.apple.com/documentation/security/keychain_services/keychain_items). A Keychain Item is made up of attributes and the data you wish to encrypt.

<!--
Diagram of: Keychain Item full of attributes
-->

The most important attribute to set is [item class](https://developer.apple.com/documentation/security/keychain_services/keychain_items/item_class_keys_and_values), using the `kSecClass` key. This tells Keychain how it should handle the item. For example, certificates do not need to be encrypted on disk since they are not secrets, but passwords do. Not setting the `kSecClass` key in your Keychain Item will result in `OSStatus` error `errSecParam`.

 Below are the available options for the `kSecClass` key:

- [kSecClassGenericPassword](https://developer.apple.com/documentation/security/ksecclassgenericpassword)
- [kSecClassInternetPassword](https://developer.apple.com/documentation/security/ksecclassinternetpassword)
- [kSecClassCertificate](https://developer.apple.com/documentation/security/ksecclasscertificate)
- [kSecClassKey](https://developer.apple.com/documentation/security/ksecclasskey)
- [kSecClassIdentity](https://developer.apple.com/documentation/security/ksecclassidentity)

In addition to telling Keychain how to handle the item, the `kSecClass` tells Keychain which attributes can be set on the Item. Setting an incorrect attribute will result in `OSStatus` error [`errSecNoSuchAttr`](./#errsecnosuchattr). If you want to see all available attributes, here is the [list](https://developer.apple.com/documentation/security/keychain_services/keychain_items/item_attribute_keys_and_values).

The two classes Cely's uses to store data are:

- `kSecClassGenericPassword`: To store a blob of data the user wishes to keep secret.
- `kSecClassInternetPassword`: To store a user credentials.

In the case your application is using Cely to handle user credentials and authentication, but also need to store certificates in the Keychain - leave Cely to handle user, and either bring in a Keychain wrapper or interface directly with Keychain. Hopefully by the end of this article, you will choose the latter.
