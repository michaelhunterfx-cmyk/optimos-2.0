# Optimos

A personal IPTV player app for your own Xtream Codes provider, built with Flutter.

This project contains the app's Dart source only (`lib/`, `pubspec.yaml`, `assets/`).
I can't run the Flutter/Android SDK toolchain from here, so you'll generate the native
Android project and build the APK yourself — it only takes a few commands.

## What's included

- `lib/main.dart` — app entry point
- `lib/theme/` — dark navy + teal design tokens
- `lib/models/` — Channel & Category data models
- `lib/services/` — Xtream Codes API client + saved-login session
- `lib/screens/` — Login, Home (hero + category rows), full Guide, Player
- `lib/widgets/` — channel card, category row
- `assets/icon/app_icon.svg` — original app icon (angular "O" mark)

## One-time setup

1. Install Flutter: https://docs.flutter.dev/get-started/install
2. Install Android Studio (for the Android SDK + an emulator, if you want one):
   https://developer.android.com/studio
3. Confirm your setup:
   ```
   flutter doctor
   ```
   Fix anything it flags (Android licenses, etc.) — it tells you the exact commands.

## Build the APK

From inside this project folder:

```bash
# 1. Generate the native android/ios folders (one-time, only if not already present)
flutter create .

# 2. Fetch dependencies
flutter pub get

# 3. Build a release APK
flutter build apk --release
```

The finished APK will be at:
```
build/app/outputs/flutter-apk/app-release.apk
```
Copy that file to your Android device (or `adb install build/app/outputs/flutter-apk/app-release.apk`
with the device connected) to install it. You'll need to allow "install from unknown sources"
since it isn't from the Play Store.

## Setting the app icon

The `assets/icon/app_icon.svg` is the original mark I designed for this app. To make it your
actual launcher icon:

1. Add `flutter_launcher_icons` to `pubspec.yaml` dev_dependencies, or simpler — export
   `app_icon.svg` to a 1024×1024 PNG (any online SVG-to-PNG tool, or Figma/Inkscape) and drop it in
   as `assets/icon/app_icon.png`.
2. Add this to `pubspec.yaml`:
   ```yaml
   dev_dependencies:
     flutter_launcher_icons: ^0.13.1

   flutter_launcher_icons:
     android: true
     ios: false
     image_path: "assets/icon/app_icon.png"
     adaptive_icon_background: "#0A1628"
     adaptive_icon_foreground: "assets/icon/app_icon.png"
   ```
3. Run:
   ```bash
   flutter pub get
   dart run flutter_launcher_icons
   ```

## Logging in

On first launch, enter your Xtream Codes provider details:
- **Host** — e.g. `http://your-provider.com:8080`
- **Username** / **Password** — as given by your provider

These are stored locally on-device only (via `shared_preferences`), never sent anywhere else.

## Notes

- This app is a player for streams you already have legitimate access to — it doesn't source
  or bundle any content itself.
- `.m3u8` (HLS) live streams are supported out of the box via `video_player`/`chewie`. If your
  provider uses a different container format for live TV, the stream URL format in
  `lib/models/channel.dart` (`Channel.fromXtream`) may need a small tweak — happy to adjust
  once you know what URLs your provider actually issues.
