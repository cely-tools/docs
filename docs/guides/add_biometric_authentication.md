<!--

https://developer.apple.com/documentation/security/keychain_services/keychain_items/restricting_keychain_item_accessibility
https://developer.apple.com/documentation/localauthentication/accessing_keychain_items_with_face_id_or_touch_id
https://developer.apple.com/documentation/localauthentication/logging_a_user_into_your_app_with_face_id_or_touch_id


Read wants to learn:

- settings toggle for `enable FaceID`
  - turned on "we get credentials and resave with faceId flag"
  - turned off "we get credentials and resave without faceId flag"
- This is for more secure apps (bank apps, enterprise app)
- handle splash screen
- Add FaceID Authentication (with allowableReuseDuration)
  - Require FaceID authentication every time
- adding plist key
> In any project that uses biometrics, include the NSFaceIDUsageDescription key in your app’s Info.plist file.


Questions that I have:

- add faceID after the fact: what happens in keychain?
- where in the API should a "with FaceID" flag exist?

TODO:
  - research existing faceID/Touch ID apps
  - setup test login view controller
  - add faceID support
  - test it out
  -

-->

<!--
In this section:
- I want to high level setup
- Mention plist
- FaceID/Touch ID is not supported with Cely's default loginViewController
-->

<!--

Flow:
  - App Loads
  - Scans for FaceID/TouchID
    - found: fill in textfields & clicks login button
    - not found: screen loads as usual
-->

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

```swift
func loginButtonClicked() {

  let username = "<USERNAME>"
  let password = "<PASSWORD>"
  let server = "some-url.com"

  // ----

  let credentialResult = Cely.credentials.set(
      username: username,
      password: password,
      server: server,
      accessibility: [.biometricsIfPossible]
  )

  switch credentialResult {
  case .success:
      Cely.changeStatus(to: .loggedIn)
  case let .failure(error):
      print("Cely store credentials error: \(error)")
  }
}
```

