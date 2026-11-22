# CH-J Tools Mobile — CH-J

CH-J Tools Mobile (previously SM Mobile) is a standalone Android and iOS/iPadOS edition of CH-J Tools, built with Flutter and Dart. It connects directly to servers over SSH and SFTP, without an account, cloud service, or telemetry. The desktop application is not a dependency.

The project is a development **alpha**. The current development version is `0.0.4`; back up important data and test on non-production servers first.

## Download and beta testing

| Platform | Where | Version and notes |
| --- | --- | --- |
| iPhone and iPad (iOS/iPadOS 17+) | [Public TestFlight beta](https://testflight.apple.com/join/hY8y48e2) | Install Apple's TestFlight app, then open the link. 0.0.4 (build 5) was uploaded on November 22, 2026 and is waiting for Apple's beta review; until it is approved, TestFlight delivers 0.0.3 (build 4). |
| Android 11+ | [Download APK](https://tools.ch-j.cz/download.php?id=bf81dd29ae06030cfba1e610&edition=mobile&lang=cs) | 0.0.4, signed alpha package for direct installation; updates 0.0.3 in place (same signing certificate) |
| Android distribution tools | [Download AAB](https://tools.ch-j.cz/download.php?id=39d5a5368f91c96daa090946&edition=mobile&lang=cs) | 0.0.4; cannot be installed directly on a phone |

Since November 7, 2026, iPhone and iPad builds are distributed only through TestFlight; the website no longer offers IPA or simulator packages. Android builds are offered on the website until the app is available on Google Play. See the [official mobile download page](https://tools.ch-j.cz/cs/?edition=mobile) for build IDs, SHA-512 values and Android signing-certificate details.

## Platforms and features

The project targets **Android 11+ (API 30+)** and **iOS/iPadOS 17+**, on phones, tablets, split-screen windows and foldables. One adaptive layout follows the window's available size in logical pixels, never a device model: narrow windows use bottom navigation, wider and short landscape windows use a navigation rail, and from 840 logical pixels the server list stays beside the selected tool. A half-folded fold or a hinge reported by the system splits the content on either side of it, and corner areas the system reserves (for example on the small display of iPhone Duo) are kept free. Rotation, resizing and folding keep the SSH session, terminal, SFTP directory and unsaved input.

Implemented in the current application (0.0.4):

- Direct SSH access with password or private-key authentication, SHA-256 host-key verification and one active connection. For host names, a server profile can use automatic address selection, IPv4 only or IPv6 only.
- An interactive PTY terminal with CTRL, ALT, ESC, TAB and arrow controls.
- SFTP browsing, upload/download, rename, directory creation and confirmed deletion, with transfer cancellation and no overwrite of existing destinations.
- A code and text editor (Re-Editor with Re-Highlight grammars) for remote files over SFTP and for files on the device (iOS/iPadOS Files and file providers, Android Storage Access Framework). It keeps encoding (UTF-8, UTF-8 BOM, UTF-16 LE/BE, Windows-1250/1252, ISO-8859-1/2), line endings and the final newline; detects changes made elsewhere before saving; saves remote files through a temporary file and an atomic rename while keeping permissions; offers find/replace with case and regular expressions, go to line, folding, JSON/YAML/XML validation and recent/favorite files. Images, safe SVG, archives (listing only), certificates (basic information only), notebooks and a hex view are shown in the app; PDF, office documents and media open in the system viewer.
- **New in 0.0.4:** saving files the SSH user may not write on Linux servers with `sudo`, using the same fixed helper as the desktop application. The sudo password is asked for each save and never stored; owner, permissions and extended attributes are kept.
- **New in 0.0.4:** System Monitor for Linux/POSIX and Windows servers: CPU, memory, swap or page files, disks, processors, GPU and network with transfer rates, a short CPU/RAM/swap history and a detailed memory breakdown (including zram and zswap on Linux). Monitoring runs only while the screen is open and the application is in the foreground.
- **New in 0.0.4:** Keychain for SSH and OpenPGP keys: import, generation with the same profiles and ratings as the desktop application (native code shared with the desktop generator), signing and verification, OpenPGP self-revocation, and keyserver lookup and explicit upload. Revealing or exporting a private key requires the Vault password.
- An AES-256-GCM encrypted local Vault with a master password of 4–64 characters and a strength indicator, password change with a new data key, Argon2id key derivation, and optional biometric unlock that starts automatically and never falls back to the device passcode. Locking disconnects SSH.
- Desktop-compatible version 1 JSON profile import/export without credentials or trusted host keys.
- On-device network diagnostics: TCP connect ping and real ICMP / ICMPv6 ping with presets or continuous runs, IPv4 + IPv6 side by side, per-attempt log, statistics (median, standard deviation, jitter, P95), RTT chart and TXT/CSV/JSON export; traceroute over ICMP, UDP and TCP (TCP on iOS/iPadOS only), with reverse DNS and comparison with the previous run. No root, server or cloud service is needed.
- **New in 0.0.4:** user interface in 42 languages, with light, dark and system themes. The license and the safety and backup guidance stay in Czech, German or English.

### Current limits and validation

There is one active SSH connection. Post-quantum SSH (ML-KEM key exchange, `ssh-mldsa44-ed25519`) is not available in the mobile SSH library: such keys can be generated and exported but not used to log in. Privileged (sudo) reading of unreadable files and editor recovery are not implemented, and the System Monitor only reads; it does not manage swap or page files. SFTP uses POSIX paths, refuses symlink traversal and existing destination files, and deletes directories non-recursively. There is no CSV import, desktop plugin system, local Hash & Checksum tool, automatic updater or background monitoring. Changed trusted host keys block connection; there is currently no UI for rotating or removing them.

Version 0.0.4 was checked on November 22, 2026 with static analysis, 239 automated tests and real OpenSSH integration tests on macOS, and installed and launched on a physical iPhone and iPad. Languages other than Czech, German and English were machine translated and have not had a native review. Android 0.0.4 has not been tested on a physical device. The [recorded test results](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FTESTING.md) in the published sources describe earlier validation.

## Official links and source code

- [CH-J Tools Mobile — official website](https://tools.ch-j.cz/cs/?edition=mobile)
- [CH-J Tools Mobile — releases and downloads](https://tools.ch-j.cz/cs/releases/?edition=mobile)
- [CH-J Tools Mobile — source code browser and source archives](https://tools.ch-j.cz/cs/source/?edition=mobile)
- [CH-J Tools Mobile — GitHub](https://github.com/ch-j-code/SM-Mobile)

This GitHub repository contains only this README. Application source code, source archives, documentation, and Android builds are published on the official website (the published source snapshot is 0.0.4); iPhone and iPad builds are distributed through TestFlight. To follow the development and build instructions below, download and extract the mobile source archive from the source browser, then open the extracted project directory.

## CH-J Ecosystem — Desktop & Mobile

CH-J Tools is the separate desktop application for Windows, macOS, and Linux. The mobile and desktop applications can each be used independently; neither requires the other.

| | CH-J Tools | CH-J Tools Mobile |
| --- | --- | --- |
| Platforms | Windows, macOS, Linux | Android, iOS/iPadOS; phones and tablets |
| Technology | Electron / JavaScript | Flutter / Dart |
| Server access | Multiple SSH sessions, terminal, SFTP and Monaco editing | One SSH connection, PTY terminal, SFTP browsing and transfers, text editor with sudo saving |
| Tools | Extended diagnostics, local hashing and first-party management plugins | Foreground System Monitor (Linux, Windows), TCP/ICMP ping and traceroute, SSH/OpenPGP Keychain |
| Profile exchange | Version 1 JSON and desktop CSV | Shared version 1 JSON only |
| Status | Development alpha | Development alpha; TestFlight beta on iPhone and iPad |
| Project README | [Desktop README](https://github.com/ch-j-code/CH-J-Servermanager#readme) | [Mobile README](https://github.com/ch-j-code/SM-Mobile#readme) |

Shared profile JSON carries only name, host, username and port. The applications use separate Vault formats; encrypted Vault files are not interchangeable.

- [CH-J Tools — official website](https://tools.ch-j.cz/cs/)
- [CH-J Tools — GitHub](https://github.com/ch-j-code/CH-J-Servermanager)
- [CH-J Tools — desktop source code](https://tools.ch-j.cz/cs/source/)

## Requirements

- Flutter with Dart compatible with the SDK constraint in `pubspec.yaml` (`>=3.10.0 <4.0.0`).
- For Android development: OpenJDK 17, Android SDK and platform tools, and an Android device or emulator. Android Studio is optional; the command-line tools can be used instead.
- For iOS development: macOS, Xcode, and an iOS device or simulator. Flutter plugins are integrated through Swift Package Manager.
- Rust (installed with `rustup`) for the native key generator; Flutter's build hook runs `tool/build_rust.py`, which installs the needed targets and builds with the locked dependencies. Android builds also need the Android NDK.
- Python 3 for the build and integration test scripts.

The configured minimum versions are Android 11 (API 30) and iOS/iPadOS 17; see the platform and validation limits above. Run `flutter doctor -v` to check your development environment before building.

## Getting started

Run the following commands from the project directory:

```sh
flutter pub get
flutter devices
flutter run -d <device-id>
```

Replace `<device-id>` with a device listed by `flutter devices`. To use an Android emulator, list available emulators with `flutter emulators` and launch one with `flutter emulators --launch <emulator-id>`. On macOS, you can open the iOS Simulator with `open -a Simulator`.

For VS Code, install the Dart and Flutter extensions, open `SM Mobile.code-workspace`, select a device in the status bar, and press F5.

## Using the application

1. Choose a language, read the security recommendations and the full CH-J license version 1.14, and explicitly accept the license.
2. Create a Vault with a master password of 4–64 characters (0.0.1 required 12–128). This password cannot be recovered. You can enable biometric unlock later in Settings.
3. Add a server. Hostnames are stored as entered; DNS results are never written back to profiles. IPv6 addresses can also be entered in square brackets.
4. Select a server and connect using a password or a securely stored private key. Private key passphrases are requested when connecting and are not stored.
5. Verify the SSH host key fingerprint through an independent channel before accepting it for the first time. A change to a known host key blocks the connection.
6. Use the terminal with a real PTY shell. CTRL and ALT apply to the next input only; ESC, TAB, and arrow keys are available on the scrollable toolbar.
7. Use Files to browse over SFTP, upload files, download through the system share/save interface, rename items, create directories, and delete items after confirmation. Directory deletion is non-recursive. Existing destination files are not overwritten; choose a different name.
8. Open a file from Files in the editor. If the SSH user may not write it on a Linux server, the app offers to save it with `sudo` after confirmation and asks for the sudo password.
9. Open Monitor to read metrics of a Linux or Windows server while that screen is visible and the application is in the foreground. Diagnostics offers TCP ping, ICMP ping and traceroute over IPv4 or IPv6, run from the device itself. TCP connect is not an ICMP ping; ping and traceroute use real ICMP / ICMPv6 through the operating system's unprivileged ICMP sockets.
10. Use Keychain to import or generate SSH and OpenPGP keys and to choose a stored SSH key in a server profile.

On phones, the main tools appear in the bottom navigation bar and the last item opens the others. Wider displays use a scrollable navigation sidebar. Settings offers 42 languages, along with light, dark, and system themes.

## Importing and exporting profiles

Settings → Import / Export profiles uses the desktop-compatible `chj-server-profiles` JSON format, version 1. Each profile contains only `name`, `host`, `username`, and `port`. Passwords, private keys, passphrases, and host key fingerprints are never transferred.

Imports are limited to 5 MiB and 10,000 records. A preview lets you skip duplicates, update existing profiles, or create copies. Updating a profile clears its credentials. Equivalent IPv6 representations are normalized only for duplicate comparison; hostnames are not resolved. Configure authentication after importing or duplicating profiles. CSV is not supported in the mobile edition.

## Building and validation

Use the build script when preparing builds to share. Each run creates a new build ID, refreshes third-party license notices, and runs static analysis and tests:

```sh
python3 tool/build.py all
```

The `all` target requires macOS and both Android and iOS toolchains. It produces the following development artifacts:

- APK: `build/app/outputs/flutter-apk/app-debug.apk`
- App bundle: `build/app/outputs/bundle/debug/app-debug.aab`
- iOS simulator application: `build/ios/iphonesimulator/Runner.app`

The APK and app bundle produced by this command are debug builds, not release packages. To build a single target, use `apk`, `aab`, or `ios-simulator` instead of `all`.

You can also run the checks and builds individually:

```sh
flutter pub get
flutter analyze
flutter test
flutter build apk --debug
flutter build appbundle --debug
flutter build ios --simulator
```

### Website distribution

```sh
python3 tool/build.py distribution
# Optional simulator build with the same build ID:
flutter build ios --simulator --debug
python3 tool/prepare_release.py
```

This target requires macOS, both toolchains, and `android/key.properties`. It creates a signed release APK and AAB plus an **unsigned** iOS device application, all with the same new build ID. The website publishes only the Android packages and source archives; iPhone and iPad builds go to TestFlight (see below). The AAB is for distribution tooling, not direct installation. Use the APK on Android.

The website offers separate Desktop and Mobile editions and a source browser with the exact mobile build snapshot. Source archives exclude signing keys, password configuration, provisioning profiles, local SDK paths, caches and build outputs. Keep the Android release keystore and its password configuration backed up privately; future Android updates must use the same signing identity.

### Android release

Create your own `android/key.properties` with `storeFile`, `storePassword`, `keyAlias`, and `keyPassword`, then run:

```sh
python3 tool/build.py aab --release
```

Signing keys and the password configuration file are excluded by `.gitignore`. Release builds do not fall back to a debug signing identity.

### iOS release

Prepare an unsigned device build:

```sh
python3 tool/build.py ios
```

Open `ios/Runner.xcworkspace` and configure your own development team, provisioning and distribution certificate. For App Store Connect and TestFlight, give every upload a new build number in `pubspec.yaml`, then build a signed IPA with the included export options:

```sh
flutter build ipa --release --export-options-plist ios/ExportOptions-TestFlight.plist
```

Upload `build/ios/ipa/*.ipa` with Xcode, Apple's Transporter app or `xcrun altool`. The scripts do not publish anything automatically.

Direct `flutter build` commands retain the existing build ID. Do not reuse that ID for a different distributed build; use `tool/build.py` to generate a new one.

## Testing

`flutter test` covers models, profile import/export, Vault cryptography, host key verification, license acceptance, application lifecycle, the editor, monitoring, diagnostics, Keychain, all 42 language catalogs and widgets. Network and device scenarios (OpenSSH, Linux and Windows servers, Internet ICMP) are skipped unless explicitly enabled. A successful standard test run does not replace these integration tests.

### Local OpenSSH integration tests

On macOS or Linux, with OpenSSH available and port 22229 free, run:

```sh
python3 tool/test_openssh.py
python3 tool/test_openssh.py --device <ios-simulator-id>
python3 tool/test_openssh.py --device <android-device-id> --android
```

These are alternative invocations for the default test runner, an iOS simulator, or an Android emulator. Replace device placeholders with the appropriate IDs.

The script creates temporary keys in a private directory, starts a server listening only on `127.0.0.1`, and stops it after testing. It does not configure the system SSH server or modify `~/.ssh`. The test account is the current local user. The fixture is not chrooted, so test operations stay within a temporary directory. Run it on a development machine. Docker is not required.

For physical devices, follow the scenarios in [Testing](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FTESTING.md) against your own isolated test server.

### Optional Linux container tests

An optional container test covers a real Linux environment, SSH password authentication, and monitoring. Docker and Colima are test tools, not dependencies of the application or its builds.

With a Docker-compatible runtime running, use:

```sh
python3 tool/test_linux.py --context <docker-context>
```

For example, on macOS with Colima installed:

```sh
colima start --profile sm-mobile-test --vm-type vz --cpu 2 --memory 2 --disk 10
python3 tool/test_linux.py --context colima-sm-mobile-test
colima stop --profile sm-mobile-test
```

The fixture listens only on `127.0.0.1:22239`, uses a randomly generated password for each run, removes the container automatically, and pins the image by digest. See the [OpenSSH image documentation](https://docs.linuxserver.io/images/docker-openssh-server/) and [Colima project](https://github.com/abiosoft/colima) for the underlying tools.

## Documentation

The following documents are available in the official mobile source browser and in the downloaded source archive:

- [Source audit](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FAUDIT.md)
- [Security](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FSECURITY.md)
- [Dependencies](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FDEPENDENCIES.md)
- [Test scenarios and results](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FTESTING.md)
- [Resolved components](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FRESOLVED_COMPONENTS.md)

## Security, backups and license

Vault encryption uses AES-256-GCM with a random data key, wrapped using a key derived from the master password through Argon2id (64 MiB, 3 iterations); Vaults created with PBKDF2-HMAC-SHA256 by 0.0.1 are upgraded after the next unlock. OS secure storage protects the encrypted envelope; optional quick unlock stores its data key separately under OS authentication controls. Private-key passphrases are not saved. Locking disconnects SSH and clears the terminal buffer. Face ID unlock was confirmed on a physical iPhone; Android biometrics remain unvalidated; see the [security model and limits](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FSECURITY.md).

Verify the first SSH host-key fingerprint through an independent channel. A changed known key blocks the connection before authentication. The master password cannot be recovered. JSON profile export is not a backup of credentials, private keys, trusted host keys or remote data. Prepare and test appropriate backups before risky operations; the application does not guarantee backup existence or recoverability.

The application displays the operational guidance and requires explicit license acceptance before first use and again for each new build. The guidance is informational and non-binding, is separate from the license, and does not require separate acceptance.

This edition is licensed under the **CH-J Proprietary Software License 1.14**. See [LICENSE](https://tools.ch-j.cz/cs/source/?edition=mobile&path=LICENSE) for the full terms and [NOTICE](https://tools.ch-j.cz/cs/source/?edition=mobile&path=NOTICE) for distribution notices.

Third-party components retain their own licenses. Full license texts are available in [Third-party notices](https://tools.ch-j.cz/cs/source/?edition=mobile&path=assets%2Flegal%2Fthird_party_notices.txt), with a component list in [Resolved components](https://tools.ch-j.cz/cs/source/?edition=mobile&path=docs%2FRESOLVED_COMPONENTS.md). They are also available in the application under Settings → Component licenses. The security recommendations are adapted from the CH-J desktop project in Czech, German, and English. The license remains in its original English; third-party rights and notices remain separate.

## Topics

`#flutter` `#dart` `#android` `#ios` `#ipados` `#ssh` `#ssh-client` `#openpgp` `#sftp` `#terminal` `#server-management` `#server-monitoring` `#network-diagnostics`
