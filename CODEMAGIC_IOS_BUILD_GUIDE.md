# Pragati Mahavidyalaya School WebView App - iOS Build Guide

## 📋 PROJECT OVERVIEW

### Project Name
**school_webview_app** - A Flutter WebView application for Pragati Mahavidyalaya Junior College

### Project Purpose
This is a mobile app that wraps the school's website into a native mobile application, providing students and parents easy access to school information.

### Live Website URL
```
https://test.webtechdomains.in/school/index.php
```

### Technology Stack
- **Framework:** Flutter 3.38.9 (Dart SDK ^3.9.2)
- **Platform:** Android ✅ Built | iOS ⏳ Pending
- **Key Package:** `webview_flutter: ^4.10.0`

---

## 📁 PROJECT STRUCTURE

```
school_webview_app/
├── lib/
│   └── main.dart              # Main app code with WebView
├── assets/
│   └── icon.jpeg              # App icon (Pragati Mahavidyalaya logo)
├── android/                   # Android configuration ✅ Complete
├── ios/                       # iOS configuration (ready for build)
│   ├── Runner/
│   │   ├── Info.plist
│   │   ├── AppDelegate.swift
│   │   └── Assets.xcassets/   # iOS icons generated
│   └── Runner.xcodeproj/
├── pubspec.yaml               # Dependencies
└── build/
    └── app/outputs/flutter-apk/
        └── app-release.apk    # Android APK (40.5MB) ✅ Built
```

---

## 📄 KEY FILES CONTENT

### pubspec.yaml
```yaml
name: school_webview_app
description: "A Flutter WebView app for Pragati Mahavidyalaya Junior College"
publish_to: 'none'
version: 1.0.0+1

environment:
  sdk: ^3.9.2

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8
  webview_flutter: ^4.10.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^5.0.0
  flutter_launcher_icons: ^0.14.3

flutter_launcher_icons:
  android: true
  ios: true
  image_path: "assets/icon.jpeg"
  min_sdk_android: 21

flutter:
  uses-material-design: true
```

### lib/main.dart
```dart
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: WebViewScreen(),
    );
  }
}

class WebViewScreen extends StatefulWidget {
  const WebViewScreen({super.key});

  @override
  State<WebViewScreen> createState() => _WebViewScreenState();
}

class _WebViewScreenState extends State<WebViewScreen> {
  late final WebViewController controller;

  @override
  void initState() {
    super.initState();

    controller = WebViewController()
      ..setJavaScriptMode(JavaScriptMode.unrestricted)
      ..loadRequest(
        Uri.parse('https://test.webtechdomains.in/school/index.php'),
      );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Pragati Mahavidyalay'),
        backgroundColor: Colors.blue,
      ),
      body: WebViewWidget(controller: controller),
    );
  }
}
```

---

## ✅ CURRENT STATUS

| Component | Status | Notes |
|-----------|--------|-------|
| Flutter Project | ✅ Complete | All files configured |
| WebView Integration | ✅ Complete | Loads school website |
| App Icon | ✅ Complete | College logo set for Android & iOS |
| Android APK | ✅ Built | 40.5MB at `build/app/outputs/flutter-apk/app-release.apk` |
| Android Testing | ✅ Tested | Runs on emulator successfully |
| iOS IPA | ❌ Pending | Cannot build on Windows - need Codemagic |
| iOS Icons | ✅ Generated | Icons placed in `ios/Runner/Assets.xcassets/` |

---

## 🎯 GOAL

Build an iOS IPA file using **Codemagic** cloud service so it can be uploaded to the Apple App Store.

---

## 📱 REQUIREMENTS FOR IOS BUILD

### What the user HAS:
- ✅ Complete Flutter project (working on Android)
- ✅ App icon configured for iOS
- ✅ GitHub account (or can create one)

### What the user NEEDS:
- ⏳ Apple Developer Account ($99/year) - https://developer.apple.com/programs/
- ⏳ Codemagic account (free) - https://codemagic.io
- ⏳ Code signing certificates and provisioning profiles

---

## 🚀 STEP-BY-STEP CODEMAGIC SETUP GUIDE

### PHASE 1: PREREQUISITES

#### Step 1.1: Create Apple Developer Account (if not done)
1. Go to https://developer.apple.com/programs/
2. Click **"Enroll"**
3. Sign in with Apple ID (or create one)
4. Pay $99/year fee
5. Wait for approval (usually 24-48 hours)

#### Step 1.2: Push Project to GitHub
1. Go to https://github.com and sign in
2. Click **"+"** → **"New repository"**
3. Name it: `school_webview_app`
4. Set to **Private**
5. Click **"Create repository"**
6. In your project folder, run these commands:
```bash
git init
git add .
git commit -m "Initial commit - School WebView App"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/school_webview_app.git
git push -u origin main
```

---

### PHASE 2: CODEMAGIC SETUP

#### Step 2.1: Create Codemagic Account
1. Go to https://codemagic.io
2. Click **"Start building now"** (top right)
3. Click **"Sign up with GitHub"**
4. Authorize Codemagic to access your GitHub
5. You're now in the Codemagic dashboard

#### Step 2.2: Add Your App to Codemagic
1. In Codemagic dashboard, click **"Add application"**
2. Select **"GitHub"** as the repository provider
3. Find and select **"school_webview_app"** repository
4. Click **"Add selected repository"**
5. Select project type: **"Flutter App"**
6. Click **"Finish: Add application"**

---

### PHASE 3: iOS CODE SIGNING SETUP

#### Step 3.1: Create App ID in Apple Developer Portal
1. Go to https://developer.apple.com/account
2. Click **"Certificates, Identifiers & Profiles"**
3. In left sidebar, click **"Identifiers"**
4. Click **"+"** button (top left)
5. Select **"App IDs"** → Click **"Continue"**
6. Select **"App"** → Click **"Continue"**
7. Fill in:
   - Description: `School WebView App`
   - Bundle ID: Select **"Explicit"**
   - Bundle ID value: `com.example.schoolWebviewApp`
8. Scroll down, click **"Continue"** → **"Register"**

#### Step 3.2: Create Distribution Certificate
1. In Apple Developer Portal, go to **"Certificates"**
2. Click **"+"** button
3. Under "Software", select **"Apple Distribution"**
4. Click **"Continue"**
5. Follow instructions to create a Certificate Signing Request (CSR):
   - On a Mac: Open Keychain Access → Certificate Assistant → Request a Certificate
   - If no Mac: Codemagic can auto-generate this (see Step 3.5)
6. Upload CSR file
7. Download the certificate (.cer file)

#### Step 3.3: Create Provisioning Profile
1. In Apple Developer Portal, go to **"Profiles"**
2. Click **"+"** button
3. Select **"App Store Connect"** (under Distribution)
4. Click **"Continue"**
5. Select your App ID: `School WebView App`
6. Click **"Continue"**
7. Select your Distribution Certificate
8. Click **"Continue"**
9. Name it: `School WebView App Distribution`
10. Click **"Generate"**
11. Download the .mobileprovision file

#### Step 3.4: Get App Store Connect API Key
1. Go to https://appstoreconnect.apple.com
2. Click **"Users and Access"** (top menu)
3. Click **"Integrations"** tab
4. Click **"App Store Connect API"**
5. Click **"+"** to generate a new key
6. Name: `Codemagic`
7. Access: **"App Manager"**
8. Click **"Generate"**
9. **IMPORTANT:** Download the .p8 file (you can only download once!)
10. Note down:
    - Issuer ID (shown at top)
    - Key ID (shown next to your key)

#### Step 3.5: Configure Code Signing in Codemagic (EASIER METHOD)
1. In Codemagic, go to your app
2. Click **"Settings"** (gear icon)
3. Scroll to **"Code signing"** section
4. Click **"iOS code signing"**
5. Choose **"Automatic"** (Codemagic manages certificates)
6. Click **"Connect"** next to App Store Connect
7. Upload your .p8 file
8. Enter Issuer ID and Key ID
9. Click **"Save"**

---

### PHASE 4: CONFIGURE BUILD SETTINGS

#### Step 4.1: Create codemagic.yaml File
Create a file named `codemagic.yaml` in the project root with this content:

```yaml
workflows:
  ios-release:
    name: iOS Release Build
    max_build_duration: 60
    instance_type: mac_mini_m2
    
    environment:
      ios_signing:
        distribution_type: app_store
        bundle_identifier: com.example.schoolWebviewApp
      flutter: stable
      xcode: latest
      cocoapods: default
      
    scripts:
      - name: Set up code signing
        script: |
          keychain initialize
          app-store-connect fetch-signing-files $(xcode-project detect-bundle-id)
          keychain add-certificates
          xcode-project use-profiles
          
      - name: Get Flutter packages
        script: flutter pub get
        
      - name: Generate icons
        script: dart run flutter_launcher_icons
        
      - name: Build iOS IPA
        script: |
          flutter build ipa --release \
            --export-options-plist=/Users/builder/export_options.plist
            
    artifacts:
      - build/ios/ipa/*.ipa
      
    publishing:
      app_store_connect:
        auth: integration
        submit_to_testflight: true
```

#### Step 4.2: Push codemagic.yaml to GitHub
```bash
git add codemagic.yaml
git commit -m "Add Codemagic build configuration"
git push
```

---

### PHASE 5: BUILD THE IPA

#### Step 5.1: Start the Build
1. In Codemagic dashboard, go to your app
2. Click **"Start new build"** (blue button, top right)
3. Select branch: **"main"**
4. Select workflow: **"iOS Release Build"**
5. Click **"Start new build"**

#### Step 5.2: Monitor Build Progress
1. Watch the build logs in real-time
2. Build typically takes 10-15 minutes
3. If successful, you'll see a green checkmark ✅

#### Step 5.3: Download IPA
1. After build completes, click **"Artifacts"** tab
2. Click to download the `.ipa` file
3. The IPA is now ready for App Store upload!

---

### PHASE 6: UPLOAD TO APP STORE

#### Step 6.1: Using Codemagic Auto-Publish (Recommended)
If you enabled `submit_to_testflight: true` in codemagic.yaml:
- The app is automatically uploaded to TestFlight
- Go to App Store Connect to submit for review

#### Step 6.2: Manual Upload (Alternative)
1. Download IPA from Codemagic artifacts
2. On a Mac, open **Transporter** app (free from Mac App Store)
3. Sign in with Apple ID
4. Drag and drop the IPA file
5. Click **"Deliver"**

---

## ⚠️ COMMON ISSUES & SOLUTIONS

### Issue 1: Bundle ID Mismatch
**Error:** "No matching provisioning profile found"
**Solution:** 
- Ensure Bundle ID in Xcode matches exactly with Apple Developer Portal
- Update `ios/Runner.xcodeproj/project.pbxproj` if needed
- In Codemagic, verify bundle identifier in environment settings

### Issue 2: Certificate Expired
**Error:** "Certificate has expired"
**Solution:**
- Create new certificate in Apple Developer Portal
- Update provisioning profile
- Re-configure code signing in Codemagic

### Issue 3: Missing Capabilities
**Error:** "Capability not registered"
**Solution:**
- For WebView apps, no special capabilities needed
- If using push notifications/etc, register in App ID settings

### Issue 4: CocoaPods Issues
**Error:** "Pod install failed"
**Solution:**
- Add to codemagic.yaml scripts:
```yaml
- name: Install CocoaPods
  script: |
    cd ios
    pod repo update
    pod install
```

---

## 📞 SUPPORT CONTACTS

- **Codemagic Documentation:** https://docs.codemagic.io/
- **Codemagic Support:** support@codemagic.io
- **Apple Developer Support:** https://developer.apple.com/support/

---

## 📝 QUICK CHECKLIST

Before building iOS IPA, ensure you have:

- [ ] Apple Developer Account (paid, $99/year)
- [ ] Project pushed to GitHub
- [ ] Codemagic account created
- [ ] App ID registered in Apple Developer Portal
- [ ] Distribution certificate created
- [ ] Provisioning profile created
- [ ] App Store Connect API key (.p8 file)
- [ ] codemagic.yaml file in project root
- [ ] Bundle ID matches everywhere

---

**Document Created:** February 4, 2026
**Project:** Pragati Mahavidyalaya School WebView App
**Platform:** Windows 11 (iOS build via Codemagic cloud)
