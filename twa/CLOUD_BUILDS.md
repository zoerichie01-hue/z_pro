Cloud build options (Codemagic, Bitrise, PWABuilder)

This project includes an `android-project/` Gradle scaffold and a `release.keystore` (generated locally).

1) Codemagic (recommended for automated, secure builds)
  - Create a Codemagic account and connect your GitHub repository (`richardzoe/z_pro`).
  - In Codemagic app settings, add these environment variables as secure (encrypted):
    - `ANDROID_KEYSTORE` : base64-encoded contents of `twa/release.keystore.b64` (or upload binary via UI)
    - `KEYSTORE_PASSWORD` : store password
    - `KEY_ALIAS` : key alias (e.g. `nova_release`)
    - `KEY_PASSWORD` : key password
  - The included `codemagic.yaml` workflow will decode the keystore, create `android-project/signing.properties`, run `./gradlew bundleRelease`, and expose the produced `.aab` as an artifact.

2) Bitrise
  - Similar flow: connect repo, create a workflow that decodes `ANDROID_KEYSTORE`, writes `signing.properties`, then runs Gradle `bundleRelease`.

3) PWABuilder / Bubblewrap cloud
  - If you prefer a fully managed PWA→Android flow, upload your PWA manifest URL or export an Android project via PWABuilder. You will still need to supply a keystore or use Google Play App Signing.

Notes & security
  - Always store keystore and passwords as encrypted secrets in CI. Do not commit raw keystores or passwords to the repo.
  - After the build completes, download the `.aab` and validate with `bundletool` before publishing.
