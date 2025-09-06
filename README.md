# WEBSPP

> **Android**: ensure `AndroidManifest.xml` contains  
> `<uses-permission android:name="android.permission.INTERNET"/>`  
>
> **iOS**: ensure `CFBundleIdentifier` and `CFBundleDisplayName` are set in Info.plist.

Commit and push to GitHub.

---

## 3. Connect Codemagic

1. Log in at [https://codemagic.io](https://codemagic.io).
2. **Add Application** → choose **GitHub** and select this repo.
3. Codemagic detects Android & iOS automatically.

---

## 4. Android Build (.apk / .aab)

1. In Codemagic → **Workflows** → **Android**.
2. Set:
   - **Gradle task**: `assembleRelease`
3. **Signing**:
   - Upload your `my-key.keystore`.
   - Enter alias + passwords.
4. **Build** → Codemagic outputs `.apk` and `.aab` under **Artifacts**.

---

## 5. iOS Build (.ipa)

1. Enroll at [Apple Developer](https://developer.apple.com/).
2. In Codemagic → **App settings** → **Apple Developer**:
   - Add your Apple ID → Codemagic can automatically fetch certs/profiles.
3. **Workflow** → **iOS**:
   - Scheme: `MyWebViewApp`
   - Export method: `Ad hoc` (for side-load) or `App Store` (for TestFlight).
4. **Build** → Codemagic outputs `.ipa` under **Artifacts**.

---

## 6. Testing

- **Android**: download the `.apk`, enable “Install from unknown sources” on your device, tap to install.
- **iOS**:
  - For Ad hoc: add device UDIDs in Apple Developer → regenerate profile.
  - Or upload `.ipa` to TestFlight via Codemagic.

---

## 7. Tips

- **App Icon**: place a generated `AppIcon.appiconset` into `ios/MyWebViewApp/Assets.xcassets/`.
- **Shrink Android Build**:
  ```gradle
  buildTypes {
    release {
      isMinifyEnabled = true
      isShrinkResources = true
      proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
    }
  }
