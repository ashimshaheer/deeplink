## Setting Up Deep Link on iOS

![App Screenshot](https://codewithandrea.com/articles/flutter-deep-links/images/associated-domains-ios.webp)

#### After add this follow the steps

Go to `ios/Runner/Runner.entitlements` add this 

```python
<string>applinks:www.domain.com</string>
```

Go to `ios/Runner/Info.plist` add this 

```python
<key>CFBundleURLTypes</key>
<array>
     <dict>
     <key>CFBundleTypeRole</key>
     <string>Editor</string>
     <key>CFBundleURLName</key>
     <string>www.domain.com</string>
     <key>CFBundleURLSchemes</key>
     <array>
     <string>https</string>
     </array>
     </dict>
</array>
<key>FlutterDeepLinkingEnabled</key>
<true/>
```

#### Server Side code

Add this in your router file `.well-known/apple-app-site-association`

```python
{
     "applinks": {
     "apps": [],
     "details": [
          {
               "appID": "TeamID.BundleID",
               "paths": ["*"]
          }
      ]
     }
}
```

After All this step your ios redirect is done

--------------------------------------------------------------------------------------------------------------------------------------------

## Setting Up Deep Links on Android

Open the `android/app/src/main/AndroidManifest.xml` file

Add an intent filter to your `activity` tag

```dart
<intent-filter android:autoVerify="true">
<action android:name="android.intent.action.VIEW" />
<category android:name="android.intent.category.DEFAULT" />
<category android:name="android.intent.category.BROWSABLE" />
<data android:scheme="https" />
<data android:host="yourDomain.com" />
</intent-filter>
<meta-data
    android:name="flutter_deeplinking_enabled"
    android:value="true" />
```

#### Android Server Verification

Add this in your router file `.well-known/assetlinks.json`

```dart
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.example",
    "sha256_cert_fingerprints":
    ["00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00"]
  }
}]
```
Create Sha256 Key use below keywords

First Go to android folder `flutter/Your-App/Android` use below command 

```dart
./gradlew signingReport
```

#### Or, if you sign your production app via Google Play Store, you can find it in the Developer Console at `Setup > App Integrity > App Signing`

![App Screenshot](https://codewithandrea.com/articles/flutter-deep-links/images/google-play-app-signing.webp)

After All this step your Android redirect is done

--------------------------------------------------------------------------------------------------------------------------------------------

## Use this package to get a path in flutter

```dart
flutter pub add go_router
```

Update your main app to like this

```dart
return MaterialApp.router(
     routerConfig: goRouter,
);
```
```dart
final goRouter = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => const HomeInfo(),
      routes: [
        GoRoute(
          path: 'details/:itemId',
          builder: (context, state) {
            debugPrint("state = ${state.uri}");
            return UniLinks(id: state.uri.toString().split('/').last);
          },
        )
      ],
    )
  ],
);
```
