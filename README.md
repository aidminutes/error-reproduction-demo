# error-reproduction-demo

Demo project in order to reproduce the SHA-1 error.

The issue currently affects two of our developers and completely blocks them, we are available for testing and further evaluation and would be happy to help debug this.

Our System Versions `yarn expo diagnostics`:

```plain
Expo CLI 2.6.12 evironment info:
  System:
    OS: macOS 10.14.1
    Shell: 5.6.2 - /usr/local/bin/zsh
  Binaries:
    Node: 8.5.0 - /var/folders/2k/xv2xh80s2xgdcygqs21trrx00000gn/T/yarn--1544616660972-0.5345639070895056/node
    Yarn: 1.12.3 - /var/folders/2k/xv2xh80s2xgdcygqs21trrx00000gn/T/yarn--1544616660972-0.5345639070895056/yarn
    npm: 5.3.0 - ~/.cache/nvm/versions/node/v8.5.0/bin/npm
    Watchman: 4.9.0 - /usr/local/bin/watchman
  IDEs:
    Xcode: 10.1/10B61 - /usr/bin/xcodebuild
  npmPackages:
    expo: ^31.0.2 => 31.0.6
    react-native: https://github.com/expo/react-native/archive/sdk-31.0.0.tar.gz => 0.57.1
```

The only difference of the two systems here being the Xcode version installed `9.2/9C40b`.

## How to reproduce

We were able to reproduce the error on a fresh setup macOS System on first try.
We installed `brew` from `brew.sh` and ran:

- `brew install git yarn nvm watchman`
- `git clone ...`
- `yarn install`
- `cd applications/demo-app`
- `yarn ios`

# 💥💥 BOOM!!! 💥💥 Error

## Synopsis

- We did try to close the iOS simulator at different times of this process to no avail.
- We also tried to reboot our systems before and after the `yarn install` as mentioned 

During our research we collected the following links and evaluated/tried their suggestions:

---

As suggested by [K999 on stackoverflow](https://stackoverflow.com/a/52766712) we 

- cleaned several temp directory paths [jump to the log](#remove-temp-dirs), 
- had `watchman` delete all its watches [jump to the log](#watchman-del-watch) and 
- we dropped all our `node_module` directories within the project [jump to the log](#drop-node-modules)
- we used the expo equivalent of `npm start -- --reset-cache` which is `yarn start --clear`. [jump to the log](#start-clear)

We also ensured `metro-react-native-babel-preset` is part of the babel preset config, which it is due to `babel-preset-expo`.

> In addition we also dropped our yarn cache [jump to the log](#drop-yarn-cache).

---

[Yairopro on stackoverflow](https://stackoverflow.com/a/53431694) suggested to update the global installation of `react-native-cli`, which in our case is not applicable as this is an expo project.

---

[expo/expo#2743](https://github.com/expo/expo/issues/2743) - SHA-1 for file ..\node_modules\expo\AppEntry.js is not computed · Issue #2743 · expo/expo

As Slapbox himself suggested removing `watchman` from the path did not solve the issue for us and the erroneous behaviour persisted.

---

[facebook/react-native#19478](https://github.com/facebook/react-native/issues/19478) - [master iOS]Metro Bundler has encountered an internal error, please check your terminal error output for more details · Issue #19478 · facebook/react-native

As suggested by handeglc we dropped our `node_module` directories throughout the project and reinstalled using `yarn` to no avail.

---

[facebook/metro#175](https://github.com/facebook/metro/issues/175) - [master iOS]Metro Bundler has encountered an internal error, please check your terminal error output for more details

We seem to have a similar issue as described here, though there have not been any actionable suggestions for us to try.

## Initial output 

After initially running `yarn start` we got the following output. The app seems to have started twice and then crashed due to SHA-1 error.


<details>
    <summary>The logs of our fresh/initial <code>yarn start</code> (click to show)</summary>

    ```plain
    • Scan the QR code above with the Expo app (Android) or the Camera app (iOS).
    • Press a for Android emulator, or i for iOS simulator.
    • Press e to send a link to your phone with email/SMS.

    Press ? to show a list of all available commands.
    Logs for your project will appear below. Press Ctrl+C to exit.
    [11:17:13] Finished building JavaScript bundle in 35347ms.
    [11:17:14] Running application "main" with appParams: {"rootTag":1,"initialProps":{"exp":{"manifest":{"splash":{"resizeMode":"contain","image":"./assets/splash.png","backgroundColor":"#ffffff","imageUrl":"http://127.0.0.1:19001/assets/./assets/splash.png"},"developer":{"projectRoot":"/Users/fst/src/github.com/aidminutes/error-reproduction-demo/applications/demo-app","tool":"expo-cli"},"loadedFromCache":false,"privacy":"public","logUrl":"http://127.0.0.1:19000/logs","orientation":"portrait","sdkVersion":"31.0.0","mainModuleName":"node_modules/expo/AppEntry","env":{},"platforms":["ios","android"],"xde":true,"id":"@zetaron/demo-app","hostUri":"127.0.0.1:19000","iconUrl":"http://127.0.0.1:19001/assets/./assets/icon.png","assetBundlePatterns":["**/*"],"name":"demo-app","slug":"demo-app","debuggerHost":"127.0.0.1:19001","icon":"./assets/icon.png","isVerified":true,"packagerOpts":{"lanType":"ip","dev":true,"minify":false,"urlRandomness":"pa-2qs","hostType":"lan"},"ios":{"supportsTablet":true},"updates":{"fallbackToCacheTimeout":0},"bundleUrl":"http://127.0.0.1:19001/node_modules/expo/AppEntry.bundle?platform=ios&dev=true&minify=false&hot=false&assetPlugin=%2FUsers%2Ffst%2Fsrc%2Fgithub.com%2Faidminutes%2Ferror-reproduction-demo%2Fnode_modules%2Fexpo%2Ftools%2FhashAssetFiles.js","version":"1.0.0"},"initialUri":"exp://127.0.0.1:19000","appOwnership":"expo","shell":0}}}. __DEV__ === true, development-level warning are ON, performance optimizations are OFF
    [11:18:50] Finished building JavaScript bundle in 64ms.
    [11:18:51] Finished building JavaScript bundle in 71ms.
    [11:18:51] Finished building JavaScript bundle in 69ms.
    [11:18:52] Finished building JavaScript bundle in 54ms.
    [11:18:52] Running application "main" with appParams: {"rootTag":11,"initialProps":{"exp":{"manifest":{"splash":{"resizeMode":"contain","image":"./assets/splash.png","backgroundColor":"#ffffff","imageUrl":"http://127.0.0.1:19001/assets/./assets/splash.png"},"developer":{"projectRoot":"/Users/fst/src/github.com/aidminutes/error-reproduction-demo/applications/demo-app","tool":"expo-cli"},"loadedFromCache":false,"privacy":"public","logUrl":"http://127.0.0.1:19000/logs","orientation":"portrait","sdkVersion":"31.0.0","mainModuleName":"node_modules/expo/AppEntry","env":{},"platforms":["ios","android"],"xde":true,"id":"@zetaron/demo-app","hostUri":"127.0.0.1:19000","iconUrl":"http://127.0.0.1:19001/assets/./assets/icon.png","assetBundlePatterns":["**/*"],"name":"demo-app","slug":"demo-app","debuggerHost":"127.0.0.1:19001","icon":"./assets/icon.png","isVerified":true,"packagerOpts":{"lanType":"ip","dev":true,"minify":false,"urlRandomness":"pa-2qs","hostType":"lan"},"ios":{"supportsTablet":true},"updates":{"fallbackToCacheTimeout":0},"bundleUrl":"http://127.0.0.1:19001/node_modules/expo/AppEntry.bundle?platform=ios&dev=true&minify=false&hot=false&assetPlugin=%2FUsers%2Ffst%2Fsrc%2Fgithub.com%2Faidminutes%2Ferror-reproduction-demo%2Fnode_modules%2Fexpo%2Ftools%2FhashAssetFiles.js","version":"1.0.0"},"initialUri":"exp://127.0.0.1:19000","appOwnership":"expo","shell":0}}}. __DEV__ === true, development-level warning are ON, performance optimizations are OFF
    [11:18:54] Finished building JavaScript bundle in 54ms.
    [11:18:55] Running application "main" with appParams: {"rootTag":21,"initialProps":{"exp":{"manifest":{"splash":{"resizeMode":"contain","image":"./assets/splash.png","backgroundColor":"#ffffff","imageUrl":"http://127.0.0.1:19001/assets/./assets/splash.png"},"developer":{"projectRoot":"/Users/fst/src/github.com/aidminutes/error-reproduction-demo/applications/demo-app","tool":"expo-cli"},"loadedFromCache":false,"privacy":"public","logUrl":"http://127.0.0.1:19000/logs","orientation":"portrait","sdkVersion":"31.0.0","mainModuleName":"node_modules/expo/AppEntry","env":{},"platforms":["ios","android"],"xde":true,"id":"@zetaron/demo-app","hostUri":"127.0.0.1:19000","iconUrl":"http://127.0.0.1:19001/assets/./assets/icon.png","assetBundlePatterns":["**/*"],"name":"demo-app","slug":"demo-app","debuggerHost":"127.0.0.1:19001","icon":"./assets/icon.png","isVerified":true,"packagerOpts":{"lanType":"ip","dev":true,"minify":false,"urlRandomness":"pa-2qs","hostType":"lan"},"ios":{"supportsTablet":true},"updates":{"fallbackToCacheTimeout":0},"bundleUrl":"http://127.0.0.1:19001/node_modules/expo/AppEntry.bundle?platform=ios&dev=true&minify=false&hot=false&assetPlugin=%2FUsers%2Ffst%2Fsrc%2Fgithub.com%2Faidminutes%2Ferror-reproduction-demo%2Fnode_modules%2Fexpo%2Ftools%2FhashAssetFiles.js","version":"1.0.0"},"initialUri":"exp://127.0.0.1:19000","appOwnership":"expo","shell":0}}}. __DEV__ === true, development-level warning are ON, performance optimizations are OFF
    [11:18:57] SHA-1 for file /Users/fst/src/github.com/aidminutes/error-reproduction-demo/applications/demo-app/.expo/__generated__/AppEntry.js is not computed
    Building JavaScript bundle [                                                                                                    ] 0%
    ```

</details>

## Steps taken to fix issue

Following we show in order the steps we took, including their output, to resolve the issue.

<a name="watchman-del-watch"></a>

```bash
error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop
➜ watchman watch-del-all
{
    "version": "4.9.0",
    "roots": [
        "/Users/fst/src/github.com/aidminutes/error-reproduction-demo",
    ]
}

error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop
➜ watchman watch-del-all
{
    "version": "4.9.0",
    "roots": []
}
```

<a name="drop-node-modules"></a>

```bash
error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop
➜ rm -rf ./node_modules ./**/node_modules ./**/.expo
```

<a name="drop-yarn-cache"></a>

```bash
error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop took 9s
➜ yarn cache clean
yarn cache v1.12.3
success Cleared cache.
✨  Done in 18.64s.
```

<a name="remove-temp-dirs"></a>

```bash
error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop took 19s
➜ rm -rf $TMPDIR/haste-* && rm -rf $TMPDIR/react-* && rm -rf $TMPDIR/metro-* && rm -rf $TMPDIR/yarn-*
```

<details>
    <summary>The logs of <code>yarn install</code> (click to show)</summary>

    ```bash
    error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop
    ➜ yarn install
    yarn install v1.12.3
    [1/4] 🔍  Resolving packages...
    [2/4] 🚚  Fetching packages...
    info @expo/traveling-fastlane-linux@1.6.2: The platform "darwin" is incompatible with this module.
    info "@expo/traveling-fastlane-linux@1.6.2" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-darwin-ia32@2.2.8: The CPU architecture "x64" is incompatible with this module.
    info "@expo/ngrok-bin-darwin-ia32@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-freebsd-ia32@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-freebsd-ia32@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-freebsd-ia32@2.2.8: The CPU architecture "x64" is incompatible with this module.
    info @expo/ngrok-bin-freebsd-x64@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-freebsd-x64@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-linux-arm@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-linux-arm@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-linux-arm@2.2.8: The CPU architecture "x64" is incompatible with this module.
    info @expo/ngrok-bin-linux-arm64@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-linux-arm64@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-linux-arm64@2.2.8: The CPU architecture "x64" is incompatible with this module.
    info @expo/ngrok-bin-linux-ia32@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-linux-ia32@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-linux-ia32@2.2.8: The CPU architecture "x64" is incompatible with this module.
    info @expo/ngrok-bin-linux-x64@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-linux-x64@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-sunos-x64@2.2.8: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-sunos-x64@2.2.8" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-win32-ia32@2.2.8-beta.1: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-win32-ia32@2.2.8-beta.1" is an optional dependency and failed compatibility check. Excluding it from installation.
    info @expo/ngrok-bin-win32-ia32@2.2.8-beta.1: The CPU architecture "x64" is incompatible with this module.
    info @expo/ngrok-bin-win32-x64@2.2.8-beta.1: The platform "darwin" is incompatible with this module.
    info "@expo/ngrok-bin-win32-x64@2.2.8-beta.1" is an optional dependency and failed compatibility check. Excluding it from installation.
    [3/4] 🔗  Linking dependencies...
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > expo > react-native-reanimated@1.0.0-alpha.10" has incorrect peer dependency "react@16.0.0-alpha.6".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > expo > react-native-reanimated@1.0.0-alpha.10" has incorrect peer dependency "react-native@^0.44.1".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > @babel/plugin-proposal-decorators@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset@0.49.2" has unmet peer dependency "@babel/core@*".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > @babel/plugin-proposal-decorators > @babel/plugin-syntax-decorators@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-proposal-export-default-from@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-syntax-dynamic-import@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-syntax-export-default-from@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-arrow-functions@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-block-scoping@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-classes@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-computed-properties@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-destructuring@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-exponentiation-operator@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-for-of@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-function-name@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-literals@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-object-assign@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-parameters@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-react-display-name@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-react-jsx@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-react-jsx-source@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-regenerator@7.0.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-runtime@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-shorthand-properties@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-spread@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-sticky-regex@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-template-literals@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-typescript@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-unicode-regex@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-react-jsx > @babel/plugin-syntax-jsx@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    warning "workspace-aggregator-3e8e6cae-4a15-4bfa-9fc4-b41bd27e8b11 > demo-app > babel-preset-expo > metro-react-native-babel-preset > @babel/plugin-transform-typescript > @babel/plugin-syntax-typescript@7.2.0" has unmet peer dependency "@babel/core@^7.0.0-0".
    [4/4] 📃  Building fresh packages...
    success Saved lockfile.
    ✨  Done in 82.68s.
    ```
</details>

```bash
error-reproduction-demo on  master [⇡] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop took 1m 23s
➜ cd applications/demo-app
```

At this point we closed iOS simulator, to ensure a clean cache on its side too.

<a name="start-clear"></a>

```bash
error-reproduction-demo/applications/demo-app on  master [⇡!] is 📦 v0.1.0 via ⬢ v8.5.0 at ☸️  docker-for-desktop took 1m 5s
➜ yarn ios --clear

...

[11:42:05] Trying to open the project in iOS simulator...
[11:42:06] Opening iOS simulator
[11:42:09] Opening exp://127.0.0.1:19000 in iOS simulator

Press ? to show a list of all available commands.
[11:42:28] SHA-1 for file /Users/fst/src/github.com/aidminutes/error-reproduction-demo/applications/demo-app/.expo/__generated__/AppEntry.js is not computed
Building JavaScript bundle [                                                                                                    ] 0%
```
