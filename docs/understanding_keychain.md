# Understanding Keychain

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

<!--

- introduction
  - shit on other frameworks
  - go over apple's security framework
- Purpose of this article
  - to demystify keychain
- Demystifying Keychain
  - explain the different classes (`kSecClass`, `kSecAttrAccount`, etc.) attached to an item
    - InternetPassword vs generic password
    - use diagram
  - OSStatus codes
- Explain the following:
  - create keychain items
  - query keychain items
    - explain `kSecReturnData as String: true`
    - find out if query returns 2 items, one of which is protected by faceID?
  - update keychain items
  - delete keychain items
- Accessibility (need to research this one more)
  - FaceID/TouchID/Passcode
    - idk how to handle passcode if user cancels?!
  - LAContext
  - Accessibility Values (`kSecAttrAccessibleWhenUnlocked`, `kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly`, etc.)

-->
