# Understanding Keychain
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
