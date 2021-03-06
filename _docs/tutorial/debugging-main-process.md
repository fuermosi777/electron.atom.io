---
version: v1.3.0
category: Tutorial
redirect_from:
    - /docs/v0.24.0/tutorial/debugging-main-process/
    - /docs/v0.25.0/tutorial/debugging-main-process/
    - /docs/v0.26.0/tutorial/debugging-main-process/
    - /docs/v0.27.0/tutorial/debugging-main-process/
    - /docs/v0.28.0/tutorial/debugging-main-process/
    - /docs/v0.29.0/tutorial/debugging-main-process/
    - /docs/v0.30.0/tutorial/debugging-main-process/
    - /docs/v0.31.0/tutorial/debugging-main-process/
    - /docs/v0.32.0/tutorial/debugging-main-process/
    - /docs/v0.33.0/tutorial/debugging-main-process/
    - /docs/v0.34.0/tutorial/debugging-main-process/
    - /docs/v0.35.0/tutorial/debugging-main-process/
    - /docs/v0.36.0/tutorial/debugging-main-process/
    - /docs/v0.36.3/tutorial/debugging-main-process/
    - /docs/v0.36.4/tutorial/debugging-main-process/
    - /docs/v0.36.5/tutorial/debugging-main-process/
    - /docs/v0.36.6/tutorial/debugging-main-process/
    - /docs/v0.36.7/tutorial/debugging-main-process/
    - /docs/v0.36.8/tutorial/debugging-main-process/
    - /docs/v0.36.9/tutorial/debugging-main-process/
    - /docs/v0.36.10/tutorial/debugging-main-process/
    - /docs/v0.36.11/tutorial/debugging-main-process/
    - /docs/v0.37.0/tutorial/debugging-main-process/
    - /docs/v0.37.1/tutorial/debugging-main-process/
    - /docs/v0.37.2/tutorial/debugging-main-process/
    - /docs/v0.37.3/tutorial/debugging-main-process/
    - /docs/v0.37.4/tutorial/debugging-main-process/
    - /docs/v0.37.5/tutorial/debugging-main-process/
    - /docs/v0.37.6/tutorial/debugging-main-process/
    - /docs/v0.37.7/tutorial/debugging-main-process/
    - /docs/v0.37.8/tutorial/debugging-main-process/
    - /docs/latest/tutorial/debugging-main-process/
source_url: 'https://github.com/electron/electron/blob/master/docs/tutorial/debugging-main-process.md'
title: "Debugging the Main Process"
sort_title: "debugging the main process"
---

# Debugging the Main Process

The browser window DevTools can only debug the renderer process scripts (i.e.
the web pages). In order to provide a way to debug the scripts from the main
process, Electron has provided the `--debug` and `--debug-brk` switches.

## Command Line Switches

Use the following command line switches to debug Electron's main process:

### `--debug=[port]`

When this switch is used Electron will listen for V8 debugger protocol
messages on the `port`. The default `port` is `5858`.

### `--debug-brk=[port]`

Like `--debug` but pauses the script on the first line.

## Use node-inspector for Debugging

**Note:** Electron doesn't currently work very well with node-inspector, and the
main process will crash if you inspect the `process` object under
node-inspector's console.

### 1. Install the [node-gyp required tools][node-gyp-required-tools]

### 2. Install [node-inspector][node-inspector]

```bash
$ npm install node-inspector
```

### 3. Install [node-pre-gyp][node-pre-gyp]

```bash
$ npm install node-pre-gyp
```

### 4. Recompile the `node-inspector` `v8` modules for Electron

**Note:** Update the target argument to be your Electron version number

```bash
$ node_modules/.bin/node-pre-gyp --target=1.2.5 --runtime=electron --fallback-to-build --directory node_modules/v8-debug/ --dist-url=https://atom.io/download/atom-shell reinstall
$ node_modules/.bin/node-pre-gyp --target=1.2.5 --runtime=electron --fallback-to-build --directory node_modules/v8-profiler/ --dist-url=https://atom.io/download/atom-shell reinstall
```

See also [How to install native modules][how-to-install-native-modules].

### 5. Enable debug mode for Electron

You can either start Electron with a debug flag like:

```bash
$ electron --debug=5858 your/app
```

or, to pause your script on the first line:

```bash
$ electron --debug-brk=5858 your/app
```

### 6. Start the [node-inspector][node-inspector] server using Electron

```bash
$ ELECTRON_RUN_AS_NODE=true path/to/electron.exe node_modules/node-inspector/bin/inspector.js
```

### 7. Load the debugger UI

Open http://127.0.0.1:8080/debug?ws=127.0.0.1:8080&port=5858 in the Chrome
browser. You may have to click pause if starting with `debug-brk` to see the
entry line.

[node-inspector]: https://github.com/node-inspector/node-inspector
[node-pre-gyp]: https://github.com/mapbox/node-pre-gyp
[node-gyp-required-tools]: https://github.com/nodejs/node-gyp#installation
[how-to-install-native-modules]: http://electron.atom.io/docs/tutorial/using-native-node-modules#how-to-install-native-modules
