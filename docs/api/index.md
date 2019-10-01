
Cely was made to help handle user credentials and handling login with ease. Below you will find documentation for Cely's Framework. **Please do not hesitate to open an issue if something is unclear or is missing.**

<!--

Variables
============


<br>
<br>
<br>

## `store`

<br>

A class that conforms to the [`CelyStorageProtocol`](#Cely.CelyStorageProtocol) protocol. By default is set to a singleton instance of `CelyStorage`.

-->


## `setup(...)`

<br>

```swift
Cely.setup(with: window, forModel: User(), requiredProperties:[.token], withOptions:[
  .homeViewController: UIHostingController(rootView: HomeContentView()),
  .loginCompletionBlock: { (username: String, password: String) in
      if username == "username" && password == "password" {
          // ...
          Cely.changeStatus(to: .loggedIn)
      }
  }
])
```

Key | Type | Required? | Description
----|------|----------|--------
`window` | `UIWindow` | ✅ | window of your application.
`forModel` | [`CelyUser`](#Cely.CelyUser) | ✅ | The model Cely will be using to store data.
`requiredProperties` | `[CelyProperty]` |  | The properties that cely tests against to determine if a user is logged in. <br> **Default value**: empty array.
`options` | `[CelyOptions: Any]` |  | Dictionary of [`CelyOptions`](./#celyoptions) to pass in additional customizations to Cely.



<br>
<br>
<br>


## `changeStatus(to:)`

<br>

Login/Logout the user.


```swift
changeStatus(to: .loggedOut) // .loggedIn
```




<br>
<br>
<br>

## `get(_:fromStorage:)`

<br>

Returns stored data for key.


```swift
let username = Cely.get(key: "username")
```



Key | Type| Required? | Description
----|------|----------|--------
`key` | String | ✅ | The key to the value you want to retrieve.
`store` | CelyStorage | no | Storage `Cely` will be using. Defaulted to `CelyStorage`




<summary>Returns</summary>

Type| Description
----|------
`Any?` | Returns an `Any?` object in the case the value nil(not found).







<br>
<br>
<br>

## `save(_:forKey:toStorage:securely:persisted:)`

<br>

Saves data in store


```swift
Cely.save("test@email.com", forKey: "email")
Cely.save("testUsername", forKey: "username", persisted: true)
Cely.save("token123", forKey: "token", securely: true)
```



Key | Type| Required? | Description
----|------|----------|--------
`value` | <code>**Any?**</code> | ✅ | The value you want to save to storage.
`key` | <code>**String**</code> | ✅ | The key to the value you want to save.
`store` | <code>**CelyStorage**</code> | no | Storage `Cely` will be using. Defaulted to `CelyStorage`.
`secure` | <code>**Boolean**</code> | no | If you want to store the value securely.
`persisted` | <code>**Boolean**</code> | no | Keep data after logout.




<summary>Returns</summary>

Type| Description
----|------
  `StorageResult` | Whether or not your value was successfully set.





<br>
<br>
<br>

## `currentLoginStatus(requiredProperties:fromStorage:)`

<br>

Will return the `CelyStatus` of the current user.


```swift
let status = Cely.currentLoginStatus()
```




Key | Type| Required? | Description
----|------|----------|--------
`properties` | [`CelyProperty`] | no | Array of required properties that need to be in store.
`store` | `CelyStorage` | no |    Storage `Cely` will be using. Defaulted to `CelyStorage`




<summary>Returns</summary>

Type| Description
----|------
[`CelyStatus`](#Cely.CelyStatus) | If `requiredProperties` are all in store, it will return `.loggedIn`, else `.loggedOut`




<br>
<br>
<br>

## `logout(usesStorage:)`

<br>

Convenience method to logout user and **remove all non-persisted data.**



```swift
Cely.logout()
```



Key | Type| Required? | Description
----|------|----------|--------
`store` | <code>**CelyStorage**</code> | no | Storage `Cely` will be using. Defaulted to `CelyStorage`.





<br>
<br>
<br>

## `isLoggedIn()`

<br>

Returns boolean whether or not the user is logged in


```swift
Cely.isLoggedIn()
```


<summary>Returns</summary>

Type| Description
----|------
`Boolean` | Returns whether or not the user is logged in




 Constants

