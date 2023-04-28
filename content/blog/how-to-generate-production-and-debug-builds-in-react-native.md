---
title: "How to generate debug and production builds for react native android"
date: 2023-04-23T18:21:57+05:30
draft: false
cover:
  image: "/blog/react-native.png"
  alt: "React Native"
  relative: false
---

### Create Production Build Android

1.  Generate a keystore

- `keytool -genkey -v -keystore your_key_name.keystore -alias your_key_alias -keyalg RSA -keysize 2048 -validity 10000`
- Move my-release-key.keystore to /android/app

1.  Update android/app/build.gradle

```
android {
....
 signingConfigs {
   release {
     storeFile file('your_key_name.keystore')
     storePassword 'your_key_store_password'
     keyAlias 'your_key_alias'
     keyPassword 'your_key_file_alias_password'
   }
 }
```

```
 buildTypes {
   release {
     ....
     signingConfig signingConfigs.release
   }
 }
}
```

1.  Create and apk

- react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res
- rm -rf ./android/app/src/main/res/drawable-\*
- rm -rf ./android/app/src/main/res/raw
- cd android
- ./gradlew assembleRelease
