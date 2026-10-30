# TV Detox contributor and AI-agent instructions

## Project

TV Detox is a no-root Android TV home-screen app and setup toolkit. Read `README.md`, the relevant shell scripts, and the Android manifest before making changes. Keep code and instructions useful across devices; never bake a TV model, IP address, local app list, account, or backup into the source tree.

The launcher declares Android min SDK 21 and target SDK 34. That does not guarantee every feature works on every television: OEMs vary in remote keys, launcher behavior, settings activities, HDMI input reporting, and Android versions. Inspect the connected device and describe any unsupported behavior instead of claiming universal compatibility.

## Safety requirements

- No rooting, bootloader unlocking, firmware flashing, factory reset, app-data clearing, or uninstalling the stock launcher.
- Treat any change to a TV or network as a separate user action. Do not run install, disable, cleanup, or reboot operations unless the user explicitly requested those actions in this conversation.
- Before an authorized update, record the device and installed package version, pull the current APK when possible, verify that the update uses the same signing key, and provide a simple rollback command/script.
- Prefer reversible updates (`adb install -r`) and preserve app data. Do not disable system packages unless the user specifically asks for cleanup and confirms the exact reviewed package list.
- Leave uncertain OEM/system packages enabled. Never promise a zero-risk or lag-free result.
- Keep device-specific app profiles, IP addresses, screenshots, APK backups, keystores, and generated builds out of Git commits.

## Build and app list

Use Java 17 and Android SDK platform 34/build-tools 34.0.0. On macOS/Linux, set `ANDROID_HOME` and run `sh build.sh` from the repository root. On Windows, use a POSIX shell such as Git Bash with the same environment. The result is `out/tvhome.apk`.

The default `apps.txt` is only a generic fallback; it is not the user's chosen profile. When helping a user set up a TV, ask which apps they want, help identify their installed package names, show the ordered list, and pass that exact list to `install.sh <TV_IP> "pkg.one,pkg.two"` or build with `APPS=/path/to/private-apps.txt sh build.sh`. Keep personal package lists outside the repository. The home screen can also receive an explicit list with the documented `--es apps` launch extra. Never silently install or publish a personal app profile.

This project supports Android TV devices through standard Android APIs, but OEM behavior varies. Do not promise it works identically on every TV; inspect the target's Android version, remote, launcher behavior, settings activities, and input reporting, and explain any limits.

The project uses a local subset of ZXing QR encoder sources under `libs/zxing-qr-src/`; keep its Apache license with the source. The full extracted vendor tree, build output, signing key, and APK files are local-only and ignored by Git.

## Change workflow

1. Inspect the current code and the requested behavior before editing.
2. Make the smallest complete change; preserve D-pad/remote navigation and provide visible focus states for TV UI.
3. Build when the user requests a build, fix, or install. Do not install on a TV unless that action is authorized.
4. Report exactly what changed, what was verified, and any limits. Never describe a build as installed or tested on hardware unless it actually was.
