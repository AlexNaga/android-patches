# Android Patches

Morphe patches for apps.

## Matlistan (`se.matlistan.free`)

| Patch | Description |
|-------|-------------|
| Enable Premium | Removes item/list limits, unlocks copy-to-list, manual sort |
| Dark mode | Dark theme for all backgrounds, dialogs, and popups |

## Prerequisites

- Java 21+
- Android SDK (`ANDROID_HOME`)
- GitHub token with `read:packages` scope (for Morphe registry)
- [Morphe CLI](https://github.com/MorpheApp/morphe-cli/releases/latest)

## Build patches

```bash
export GITHUB_TOKEN=$(gh auth token)
export ANDROID_HOME=~/android-sdk

./gradlew build
```

Output: `patches/build/libs/patches-<version>.mpp`

## Patch an APK

```bash
# Extract base.apk from XAPK
unzip Matlistan_X.Y.Z.xapk -d work/

# Patch
java -jar morphe-cli.jar patch \
  --patches patches/build/libs/patches-*.mpp \
  work/base.apk -o patched.apk
```

## Install (with split APKs)

```bash
# Re-sign everything with same key
apksigner sign --ks key.jks --ks-pass pass:password patched.apk
for f in work/split_*.apk; do
  apksigner sign --ks key.jks --ks-pass pass:password --out "$f" "$f"
done

# Install via ADB session
adb push patched.apk /data/local/tmp/base.apk
adb push work/split_*.apk /data/local/tmp/

SESSION=$(adb shell pm install-create -r | grep -oP '\d+')
adb shell pm install-write $SESSION base /data/local/tmp/base.apk
for i in 0 1 2 3; do
  adb shell pm install-write $SESSION split_$i /data/local/tmp/split_$i.apk
done
adb shell pm install-commit $SESSION
```

## Alternative: Morphe Manager (on-device)

Open this link on your phone to add the source in Morphe Manager:
https://morphe.software/add-source?github=AlexNaga/android-patches

Or copy the `.mpp` file to your phone and load it as a local patch source.
