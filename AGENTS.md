This is a soft fork of github.com/gwitko/Conduit (`origin` is janbam/Conduit, `upstream` is the original). Keep the fork's master in sync with upstream; development happens here, publishing happens there.

- Android development builds only. No release signing, no Play uploads, no iOS work (can't build/sign iOS on Linux anyway).
- Flutter lives at `~/flutter` and may not be on PATH; prefix commands with `PATH="$HOME/flutter/bin:$PATH"` or export it.
- Build with `flutter build apk --debug --flavor <play|full>`; `play` is the Play variant, `full` is the GitHub/F-Droid variant with broader storage access.
- Debug builds are auto-signed by Gradle; never create `android/key.properties` or a release keystore in this checkout.
- Deps are Dart/pub (`flutter pub get`), not npm. Android SDK is at `~/Android/Sdk`.
