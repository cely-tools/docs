# Overview

<!--
THIS IS THE FIRST ENCOUNTER WITH THE PROJECT! NEEDS TO BE FAST!
-->

<!-- Add install snippet here -->

## Cely Setup
Let's start by creating a `User` model that conforms to the [`CelyUser`](http://celylog.in/api/#celyuser) Protocol:


```swift
// User.swift

import Cely

struct User: CelyUser {

  enum Property: CelyProperty {
      case token = "token"
  }
}
```

With the recent changes made to the [App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle), depending on what version of iOS your application will support, iOS 12 and earlier or iOS 13 and later — you will need to call `Cely.setup(_:)` in different parts of your app's codebase.

The rest of the guide will follow as if your application supports iOS 13 & later. In the next section we will shift our focus to `scene(_:willConnectTo:options:)`


```swift
// iOS 13 | Swift 5.0 | Xcode 11.0
import Cely

class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        if let windowScene = scene as? UIWindowScene {
            let window = UIWindow(windowScene: windowScene)
            self.window = window
            window.makeKeyAndVisible()

            Cely.setup(with: window, forModel: User(), requiredProperties: [.token], withOptions: [
                .homeViewController: UIHostingController(rootView: HomeContentView())
            ])
        }
    }
}
```
> Cely has a few defaults, one of which is to load a built-in `LoginViewController` to help with speedy development. If you would like to use your own login view controller checkout [setup own `LoginViewController`](/usage/advance_usage/#loginviewcontroller).

Now hit **RUN** on Xcode. You should see Cely's default Login Screen (below):

![](../images/getting_started_stage_0.png)


## Save Credentials

When the user clicks the **Login** button, we need away to retrieve the `username` and `password`, we do that by using the `.loginCompletionBlock` option:

```swift
Cely.setup(with: window, forModel: User(), requiredProperties: [.token], withOptions: [
    .homeViewController: UIHostingController(rootView: HomeContentView()),
    .loginCompletionBlock: { (username: String, password: String) in
        if username == "hello" && password == "world" {
            Cely.save("FAKETOKEN-\(username)\(password)", forKey: "token", securely: true)
            Cely.credentials.set(username: username, password: password, server: "api.example.com")
            Cely.changeStatus(to: .loggedIn)
        }
    }
])
```

Now hit **RUN** again on Xcode, you should be brought back to Cely's built-in `LoginViewController`. Set the credentials below:

- username: `hello`
- password: `world`


Now click the **Login** button, this time you should be redirected to your application's _(entry on `Main.storyboard`)_.

![](../images/getting_started_first_login.gif)




## What's next?

- [Set custom `LoginViewController`](/usage/advance_usage/#loginviewcontroller)
