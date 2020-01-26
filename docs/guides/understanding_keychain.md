# Understanding Keychain

> **This is a living document, so expect updates in the future. 📖**

## Introduction

{!docs/includes/keychain-wrapper-usage.md!}

> *Now why is this article even necessary when the user can just use the Cely framework?*

We feel Cely should be considered as an end solution for 99% of codebases that store credentials securely and have a login system. It's the remaining 1% of codebases that require a small tweak that this article is targeting. We've seen many frameworks change their API only to accommodate a small fraction of their userbase. Given the remaining 1%, each requiring different solutions, trying to solve everyone's problem is not the approach the Cely team wants to take moving forward. Instead, we'd like to enable developers to build out this remaining 1% themselves.

By the end of this article — we hope the reader should have a firm grasp of [Keychain Services](https://developer.apple.com/documentation/security/keychain_services) and what all that entails. We will cover Keychain Services in depth, going over simple CRUD operations, ACL, biometrics, etc.

{!docs/includes/understanding_keychain/0_keychain-anatomy.md!}

## Keychain Operations

In the following sections we will explain how to perform CRUD operations to Keychain items and what are some common gotchas.



{!docs/includes/understanding_keychain/1_create-item.md!}
{!docs/includes/understanding_keychain/2_get-item.md!}
{!docs/includes/understanding_keychain/3_update-item.md!}
{!docs/includes/understanding_keychain/4_delete-item.md!}
{!docs/includes/understanding_keychain/5_accessibility.md!}
{!docs/includes/understanding_keychain/6_access-control.md!}
{!docs/includes/understanding_keychain/7_common-errors.md!}
