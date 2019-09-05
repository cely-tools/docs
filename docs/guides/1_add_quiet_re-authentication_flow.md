# Introduction

Quiet Re-Authentication allows your application to reauthenticate in the background without requiring the user to login. Below is a diagram illistrating the steps your application should take when adopting this flow.

*Quiet Re-Authentication Flow*
![](../images/guides/quiet_re-authentication_flow.jpg)


## Quiet Re-Authentication with Cely

In the next few sections were going to be going over Cely's role/responsibility in this flow and how to adopt it into your application.

## App Launch Flow

<!--
In this section:
I want to go over the "app launch flow with cely" diagram.
-->


![](../images/guides/quiet_re-authentication_flow-with_cely-app_launch_flow.jpg)


#### Cely Responsibility

<!--
In this section:
I want to go over the Cely's responsibility during the "app launch flow"
-->

With the recent changes made to the [App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle), depending on what version of iOS your application will support, you will need to call `Cely.setup(_:)` in different parts of your app's codebase. If your application supports iOS 12 or earlier, `Cely.setup(_:)` will be called in `application:didFinishLaunchingWithOptions` inside of `AppDelegate.swift`. If your application supports iOS 13 and later, `Cely.setup(_:)` will be called in `scene:willConnectTo:options:` inside of `SceneDelegate.swift`.

The rest of the guide will follow as if your application supports iOS 13 & later.


```swift
// iOS 13 | Swift 5.0 | Xcode 11.0
import Cely

struct User: CelyUser {

  enum Property: CelyProperty {
    case token = "token"
  }
}

class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        if let windowScene = scene as? UIWindowScene {
            let window = UIWindow(windowScene: windowScene)
            self.window = window
            window.makeKeyAndVisible()

            Cely.setup(with: window, forModel: User(), requiredProperties: [.token], withOptions: [
                .homeViewController: UIHostingController(rootView: HomeContentView()),
                .loginViewController: UIHostingController(rootView: LoginContentView())
            ])
        }
    }
}
```

As a brief explanation, we pass your application's `UIWindow` to give Cely access to switch inbetween your Login and Home Screen. Next, we pass an instance of our `User` model which contains the `Property` enum. Finally, using the `requiredProperties` parameter, we tell Cely what properties are required in order to continue to the `.homeViewController`. In this example, if Cely does not find a property `.token` in its storage, or if `requiredProperties` is empty, Cely will redirect the user to `.loginViewController`.


#### Developer Responsibility

<!--
In this section:
I want to go over the Developer's responsibility during the "app launch flow"
-->

Below is an example what is required of the developer to implement in order to complete the App Launch Flow for Quiet Re-Authentication. We will finish writing `sceneWillEnterForeground(_:)` in the **Expired Token Flow**.

```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    ...

    func sceneWillEnterForeground(_ scene: UIScene) {
        guard let token = Cely.get("token") as? String else { return Cely.logout() }

        LoginService.status(for: token) { result in
            switch result {
            case .success:
            case .failure(let error as HTTPError) where error == .unauthorized:
                // TODO: Revisit in Expired Token Flow
            }
        }
    }
}
```

```swift
class LoginService {
    static func status(for token: String, completionHandler: @escaping (Result<Void?, Error>) -> Void) {
        // make API call to check token status
        completionHandler(someResult)
    }
}
```



## Login Flow
![](../images/guides/quiet_re-authentication_flow-with_cely-login_flow.jpg)

When the user successfully logs in, ensure the user's token is saved to keychain using `Cely.save(_:)` before calling the method `Cely.changeStatus(_:)` which will redirect the user to the home screen. Below is a pseudo code example:

```swift

let username = usernameTextField.text
let password = passwordTextField.text

Login.Service(username: username, password: password)
```

```swift
class LoginService {

    ...

    static func login(username: String, password: String) {
        API.login(username: username, password: password) { result in
            switch result {
            case .success(let token):
                if Cely.save(token, forKey: "token", securely: true) == .success {
                    Cely.credentials.set(
                        username: username,
                        password: password,
                        server: "api.example.com"
                    )
                    Cely.changeStatus(to: .loggedIn)
                }
            case .failure(let error):
                // handle error
            }
        }
    }
}
```

Though a built-in `LoginViewController` is provided by Cely, as of Cely v3, it is encouraged for this built-in controller to only be used for rapid development/prototyping and not production.


## Expired Token Flow

![](../images/guides/quiet_re-authentication_flow-with_cely-expired_token_flow.jpg)

If you come from a Backend development background, the idea of implementing a [refresh token](https://auth0.com/learn/refresh-tokens/) to handle re-authentication may see like the best option, but its going to cost a bit of overhead and precious developer time. Instead, because of the security Keychain Services provides, we are able to [store users credentials directly onto the device](https://developer.apple.com/documentation/security/keychain_services/keychain_items/adding_a_password_to_the_keychain), making re-authentication extremely easy. How does Cely help us do this?

Up until this point we have stored the API token in Keychain Services using `Cely.save(_:)`, but it is expected for this token to eventually expire. When it does expire, we will be receiving `401` errors from our API. Since we are following the *Quiet Re-Authentication Flow*, our application will re-authenticate the user instead of logging them out.

There are two places where we need to handle Re-authentication.

- When the app starts up, *(if the user is already logged in)*
- When an API responds with `401` error

Below is a pseudo code example for when the application starts up:

```swift

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    ...

    func sceneWillEnterForeground(_ scene: UIScene) {

        // ping a `/status` endpoint to determine if token is still valid
        // 401 error occurred:
        let credentials = Cely.credentials.get()
        LoginService.reAuthenticate(username: credentials.username, password: credentials.password) {
           //...
        }
    }
}
```

```swift
class LoginService {

    ...

    static func reAuthenticate(username: String, password: String, completionHandler: @escaping (Result<Void?, Error>) -> Void) {
        API.login(username: username, password: password) { (result) in
            switch result {
            case .success(let token):
                if Cely.save(token, forKey: "token", securely: true) == .success {
                    return completionHandler(.success(nil))
                }
            case .failure(let error):
                // handle error
                // Log user out
            }
        }
    }
}
```

It is up to the developer's discretion on how the second example, *API responds with `401` error*, will be architected. But essentially, you will need to do the following:

- Intercept a failed response from an API request
- Re-Authenticate user
    - on success:
        - re-request failed API request and continue original call sequence
    - on failure:
        - log user out


```swift

class SomeService {

    static func getSomeData(with id: String, completionHandler: @escaping (Result<Void?, Error>) -> Void) {
        API.someEndpoint(id: id) { (result) in
            switch result {
            case .success(let data):
                // do something with data
            case .failure(let error as HTTPError) where error == .unauthorized:
                let credentials = Cely.credentials.get()
                return LoginService.reAuthenticate(username: credentials.username, password: credentials.password) { (result) in
                    switch result {
                    case .success(let token):
                        if Cely.save(token, forKey: "token", securely: true) == .success {
                            return SomeService.getSomeData(with: id, completionHandler: completionHandler)
                        }
                    case .failure(let err as HTTPError) where err == .unauthorized:
                        // handle error
                        // Log user out
                    }
                }
            }
        }
    }
}
```
Upon success, since the credentials used to re-authenticate are still valid, **only** update the `token` to avoid multiple expensive calls.

## Conclusion

In conclusion, with this guide you should be given a high level overview of how to implement Quiet Re-Authentication in your application using Cely. This document is a living document so if something is not clear or if you feel we are missing something, please open up an issue on this repo. The Cely team values documentation above all, so your help to improve it would greatly be appreciated 😀.
