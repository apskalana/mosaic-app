# MosAIc v3

Interactive university prototype, with a static web application and an Android
WebView application. The source was imported from the supplied MosAIc v3 archives.

https://mosaic-app-kalana11.vercel.app/

## Project layout

- `web/`: web application, PWA manifest, and service worker.
- `android/`: Android application, including its bundled HTML application.
- `.github/workflows/build-apk.yml`: debug APK build and artifact upload.

The web application and Android HTML are separate copies. When updating the UI,
update both `web/index.html` and `android/app/src/main/assets/index.html`.

## Run the web application

From the repository root:

```sh
python3 -m http.server 8080 --bind 127.0.0.1 --directory web
```

No package installation or web compilation is required. Serve the files over HTTP
so the manifest and service worker can load. See `web/README.md` for prototype
features.

## Build the Android debug APK

Requirements: JDK 17, Android SDK platform 35, Android build tools 35.0.0, and
`ANDROID_HOME` pointing to the SDK. The checked-in wrapper uses Gradle 8.9 with a
verified distribution checksum; Android Gradle Plugin is 8.7.3.

```sh
cd android
./gradlew --no-daemon --max-workers=2 assembleDebug
```

The APK is written to `android/app/build/outputs/apk/debug/app-debug.apk`. It uses
the standard debug signing key and supports Android 7.0 (API 24) and newer. It can
be installed on a connected device with:

```sh
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

Run Android static analysis with `./gradlew --no-daemon --max-workers=2 lintDebug`.

## GitHub Actions

The **Build MosAIc APK** workflow runs for Android/workflow changes on `main` and
pull requests, and can also be started manually from the Actions tab. Open a
successful run and download **MosAIc-v3-debug-apk** from its artifacts. The artifact
contains `app-debug.apk` and is retained for 14 days. No signing secrets are needed
for this debug build.

The login is a local demonstration and is not production authentication. The app
uses synthetic prototype data and stores demonstration account information in
the browser or Android WebView.
