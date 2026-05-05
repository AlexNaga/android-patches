# Android Patches

![Release](https://img.shields.io/github/actions/workflow/status/AlexNaga/android-patches/release.yml)
![License](https://img.shields.io/github/license/AlexNaga/android-patches)

Morphe patches for apps I use.

| App | Package | Patches |
|-----|---------|---------|
| Matlistan | `se.matlistan.free` | <ul><li>Enable Premium</li><li>Dark mode</li></ul> |

## How to use

Install [Morphe Manager](https://morphe.software/), then open this link on your phone to add the source:

https://morphe.software/add-source?github=AlexNaga/android-patches

Alternatively, add `https://github.com/AlexNaga/android-patches` as a remote source in Morphe Manager.

## Build from source

Requirements: Java 21+, Android SDK (`ANDROID_HOME`), GitHub token with `read:packages` scope.

```bash
export GITHUB_TOKEN=$(gh auth token)
./gradlew :patches:buildAndroid generatePatchesList
```

Output: `patches/build/libs/patches-<version>.mpp`

To use the local `.mpp`: copy it to the device and load it as a Local source in Morphe Manager.
