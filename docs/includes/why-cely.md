## Why Cely

Cely's goal is to add a secure login system into your app in under 30 seconds!

### Background

{!docs/includes/keychain-wrapper-usage.md!}

This is no coincidence either, Keychain Service is apart of Apple's lower level [Security Framework](https://developer.apple.com/documentation/security) which is written in C. I too would encourage you to use a framework instead of interfacing with this API directly. There's no need to reinvent the wheel when working with this API. But if you must, I suggest you bookmark [osstatus.com](https://www.osstatus.com/search/results?platform=all&framework=Security&search=#) to help you decipher all the possible `OSStatus` error codes your app will encounter.

But **storing information securely is only one half of the solution**, the **other half is building a Login system**. This Login system will be an entirely different part of your codebase that will be responsible for interacting with your application's `UIWindow`, handle abrupt login status change (redirect to login screen), update Keychain, etc.

This is where Cely comes in, it solves both problems by combining these two entirely different bits of functionality into one seamless all-in-one solution.

