# Instagram Archive Studio standalone port – DEVELOPMENT SOURCE ONLY

This source is a best-effort standalone Android port of the actual V1.5 source found in the user's Library. It is **not a built, verified APK**. No Termux or external server is required in the intended design. Chaquopy embeds Python into the APK; the Python engine serves the original dashboard on localhost, started by MainActivity.

## Build prerequisites

* JDK 17 or 21, Android SDK platform 35/build-tools 35, Gradle 8.11.1, internet or cached Maven dependencies for AGP 8.9.2 and Chaquopy 17.1.0.
* From this directory run: `gradle assembleDebug` (or use Android Studio build). Install `app/build/outputs/apk/debug/app-debug.apk` after a successful build.
* Chaquopy will fetch/gallery-dl and Instaloader through pip during the build. Runtime network access is still required to access Instagram.

## Porting performed

* Uses original V1.5 HTML/CSS/JS and Python backend, with platform substitutions.
* Uses Python embedded by Chaquopy (arm64), a Java WebView, on-device localhost HTTP server.
* Converts gallery-dl child-process invocation to in-process library invocation while preserving CLI arguments and downloaded-file names.
* Android's native document picker handles cookies.txt imports.
* Logs, media selection, password/2FA, ZIP export and API endpoints preserved in code, subject to device-level testing.
* Saves completed exports to visible Downloads/Instagram Archive using MediaStore; working files are under app-specific external storage.

## Known gaps – do not describe as full parity until tested and fixed

* Browser private cookies cannot be read across apps on modern Android; import cookies.txt or use password/2FA. Old browser buttons are disabled.
* Stop checks run between downloaded media; an ongoing network request can continue to the HTTP timeout.
* App does not have a foreground download service yet, so Android may suspend/kill long operations in background.
* Android content picker cookies import and media/ZIP export are unverified on device.
* MediaStore export can require additional collision/filename handling in some Android versions.
* No authenticated downloads were tested; Instagram can independently reject even public profile requests, or issue HTTP 429.
* `gallery_dl.main()` invocation changes `sys.argv` only while one download is running; confirm its exact behavior for the pinned version.
* No new APK has been built or signed: Maven build dependencies are unavailable in the current offline container.

Security: no user cookies or passwords are bundled in this source project. Keep cookies files private. Instagram access must follow user permissions and applicable terms.
