# jiroikedafeingold.github.io

GitHub Pages **user site** for `jiroikedafeingold`. It exists for one reason: to serve
files that Android and iOS require at the **root of the domain**, which project-site
repos (served under `/<repo>/`) cannot do.

## What's here

| Path | Purpose |
|---|---|
| `.well-known/assetlinks.json` | Android **Digital Asset Links** — lets Android verify that this domain and the Star Battle Nova Android app belong to the same owner, so `https://jiroikedafeingold.github.io/StarBattle/s.html?b=…` share links open the app directly instead of showing an "open with" chooser. |
| `.nojekyll` | Stops GitHub Pages running Jekyll, which would otherwise **silently drop `.well-known/`** (Jekyll ignores dot-directories). Without this file the assetlinks URL 404s. |
| `index.html` | A minimal landing page so the bare domain isn't blank. |

Project sites are unaffected: `https://jiroikedafeingold.github.io/StarBattle/` keeps
serving `docs/` from the `StarBattle` repo.

## Finishing the Android App Links setup

`sha256_cert_fingerprints` is currently a **placeholder**, so verification will fail
(harmlessly — links fall back to the chooser). To finish:

1. Upload the first Android build to Play Console.
2. Go to **Play Console → Test and release → Setup → App integrity → App signing**.
3. Copy the **SHA-256 certificate fingerprint** of the *app signing key* (not the upload
   key — Play re-signs your app, so the upload key's fingerprint is the wrong one).
4. Paste it here, replacing the placeholder, and push.
5. Verify: `https://jiroikedafeingold.github.io/.well-known/assetlinks.json` returns the
   JSON with `Content-Type: application/json`, then on a device run
   `adb shell pm verify-app-links --re-verify com.jirofeingold.starbattlenova` and check
   `adb shell pm get-app-links com.jirofeingold.starbattlenova`.

For local debug builds, add a second entry with the debug keystore fingerprint from:

```
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey \
        -storepass android -keypass android
```
