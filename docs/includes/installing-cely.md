
## Requirements

- iOS 11.0+
- Xcode 9.0+
- Swift 4.0+

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that automates the process of adding frameworks to your Cocoa application.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate Cely into your Xcode project using Carthage, specify it in your `Cartfile`:

```bash
github "cely-tools/Cely" ~> 3.0.0-alpha.1
```

### CocoaPods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects. You can install it with the following command:

```bash
$ gem install cocoapods
```

To integrate Cely into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
use_frameworks!

# Alpha builds are not deployed to cocoapods
pod 'Cely', '~> 3.0'
```

Then, run the following command:

```bash
$ pod install
```
