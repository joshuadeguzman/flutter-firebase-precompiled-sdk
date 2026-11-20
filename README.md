# flutter-firebase-precompiled-sdk

A demo app showing how to cut Flutter iOS build times when using Cloud Firestore. It's the companion code for the blog post [Improve Flutter iOS build time when using Firestore](https://joshuamdeguzman.com/improve-flutter-ios-build-time-when-using-firestore/).

## The problem

The Firestore iOS SDK pulls in a large amount of C++ code that CocoaPods compiles from source. On a clean build this can add several minutes.

## The fix

Use the precompiled Firestore frameworks from [invertase/firestore-ios-sdk-frameworks](https://github.com/invertase/firestore-ios-sdk-frameworks). Add this to the `Runner` target in `ios/Podfile`:

```ruby
target 'Runner' do
  use_frameworks!
  use_modular_headers!

  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))

  pod 'FirebaseFirestore', :git => 'https://github.com/invertase/firestore-ios-sdk-frameworks.git', :tag => '9.2.0'
end
```

The tag has to match the Firebase iOS SDK version used by your `cloud_firestore` plugin.

## Branches

- `joshua/precompiled-tests`: builds with the precompiled SDK.
- `joshua/no-precompiled-tests`: builds from source, for comparison.

## Running it

```bash
flutter pub get
cd ios && pod install && cd ..
flutter run
```

You'll need your own Firebase project and `GoogleService-Info.plist`. Set them up with `flutterfire configure`.
