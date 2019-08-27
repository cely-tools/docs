<div style="text-align:center"><img src ="Images/READMEHeader.png" /></div>

![Swift 4.0](https://img.shields.io/badge/Swift-3.0-orange.svg?style=flat)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![CocoaPods compatible](https://img.shields.io/cocoapods/v/Cely.svg)](https://cocoapods.org/pods/Cely)
[![Cookiecutter-Swift](https://img.shields.io/badge/cookiecutter--swift-framework-red.svg)](http://github.com/cookiecutter-swift/Framework)
[![Build Status](https://app.bitrise.io/app/aff729145cb46dfe/status.svg?token=YUV0bymd7P_w2tdiKw2xOQ&branch=master)](https://app.bitrise.io/app/aff729145cb46dfe)
[![codecov](https://codecov.io/gh/initFabian/Cely/branch/master/graph/badge.svg)](https://codecov.io/gh/initFabian/Cely)
> Prounounced Cell-Lee

Cely’s goal is to add a login system into your app in under 30 seconds!

- [Overview](#overview)
  - [Background](#background)
    - [Details:](#details)

## Overview
Cely's goal is to add a login system into your app in under 30 seconds!

<img style="height: 600px" src ="Images/background_cely_login.gif" />

### Background
Whether you're building an app for a client or for a hackathon, building a login system, no matter how basic it is, can be very tedious and time-consuming. Cely's architecture has been battle tested on countless apps, and Cely guarantees you a fully functional login system in a fraction of the time. You can trust Cely is handling login credentials correctly as well since Cely is built on top of [Locksmith](https://github.com/matthewpalmer/Locksmith), swift's most popular Keychain wrapper.

#### Details:
What does Cely does for you?

1. Simple API to store user creditials and information **securely**
 - `Cely.save("SUPER_SECRET_STRING", forKey: "token", securely: true)`
2. Manages switching between your app's Home Screen and Login Screen with:
 - `Cely.changeStatus(to: .loggedIn) // or .loggedOut`
3. Customizable starter Login screen(or you can use your login screen)

What Cely **does not do** for you?

1. Network requests
2. Handle Network errors
3. Anything with the network

