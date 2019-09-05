
(has read _I want to add single user basic authentication_)
(has read _understanding keychain_)

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
