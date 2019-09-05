<!--

Keychain Notes:

- kSecAttrAccessibleWhenUnlocked
  - User unlocks phone -> Project can read passcode (in background)
  - User locks phone -> Project can NO LONGER READ passcode (in background)
- kSecAttrAccessibleAfterFirstUnlock
  - User unlocks phone once -> Project can continue reading passcode (in background)
  - User locks phone -> Project can continue reading passcode (in background)
- kSecAttrAccessibleWhenUnlockedThisDeviceOnly
- kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly

- kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly
  - User unlocks phone -> Project can read passcode (in background)
  - User locks phone -> Project can NO LONGER READ passcode (in background)
  - Will only allow you to store items in keychain if a passcode is set
  - If device has NO PASSCODE: Cely will return Error
  - Items that get stored when passcode is set -> Remove passcode from device:
    - Secure erase the key that are protecting keychain items. "Cryptographically preventing us from ever decrypting those keychain items again"
-->

# Understanding Keychain

> **This is a living document, so expect updates in the future. 📖**

## Introduction

{!docs/includes/keychain-wrapper-usage.md!}

> *Now why is this article even necessary when the user can just use the Cely framework?*

Cely should be considered as an end solution for 99% of codebases that store credentials securely and have a login system. It's the remaining 1% of codebases that require a small tweak that this article is targeting. We've seen many frameworks change their API only to accommodate a small fraction of their userbase. Given the remaining 1%, each requiring different solutions, trying to solve everyone's problem is not the approach the Cely team wants to take moving forward. Instead, we'd like to enable developers to build out this remaining 1% themselves.

By the end of this article — the reader should have a firm grasp of [Keychain Services](https://developer.apple.com/documentation/security/keychain_services) and what all that entails. We will cover Keychain Services in depth, going over simple CRUD operations, ACL, biometrics, etc.

## Keychain Anatomy

<!--
Diagram of: Keychain database full of Keychain Items
-->

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

In addition to telling Keychain how to handle the item, the `kSecClass` tells Keychain which attributes can be set on the Item. Setting an incorrect attribute will result in `OSStatus` error `errSecNoSuchAttr`. If you want to see all available attributes, here is the [list](https://developer.apple.com/documentation/security/keychain_services/keychain_items/item_attribute_keys_and_values).

The two classes Cely's uses to store data are:

- `kSecClassGenericPassword`: To store a blob of data the user wishes to keep secret.
- `kSecClassInternetPassword`: To store a user credentials.

In the case your application is using Cely to handle user credentials and authentication, but also need to store certificates in the keychain - leave Cely to handle user, and either bring in a Keychain wrapper or interface directly with Keychain. Hopefully by the end of this article, you will choose the latter.

## Keychain Operations

Below are sections explaining how to perform CRUD operations to Keychain.

### Writing Item

<!--
In this section: I want to go over how to use `SecItemAdd(_:)`
and what happens if you try to create something that already exists - I actually dont know 😬

https://developer.apple.com/documentation/security/keychain_services/keychain_items/adding_a_password_to_the_keychain
-->

### Querying Item

<!--
In this section:
- I want to go over how to use `SecItemCopyMatching(_:)`
- explain `kSecReturnData as String: true`

https://developer.apple.com/documentation/security/keychain_services/keychain_items/searching_for_keychain_items
-->

### Updating Item

<!--
In this section: I want to go over how to use `SecItemUpdate(_:)`
and what happens if you try to update something that doesnt exist

https://developer.apple.com/documentation/security/keychain_services/keychain_items/updating_and_deleting_keychain_items
-->

### Deleting Item

<!--
In this section: I want to go over how to use `SecItemDelete(_:)`
and what happens if you try to delete something that doesnt exist

https://developer.apple.com/documentation/security/keychain_services/keychain_items/updating_and_deleting_keychain_items
-->


## Accessibility (need to research this one more)

<!--

- Querying Keychain and returns 2 items, one of which is protected by faceID. - What happens?
- FaceID/TouchID/Passcode
  - idk how to handle passcode if user cancels?!
- LAContext
- Accessibility Values (`kSecAttrAccessibleWhenUnlocked`, `kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly`, etc.)
-->
