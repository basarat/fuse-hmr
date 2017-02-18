# Fuse HMR
A customizable HMR plugin for [FuseBox](http://fuse-box.org/) 🌹

> Built with TypeScript, for TypeScript ❤️️

## Install
```
npm install fuse-box fuse-hmr
```

## Usage

### `setStatefulModules(...moduleNames:string)`

```
import {setStatefulModules} from 'fuse-hmr';
setStatefulModules('foo','bar');
```

On an HMR update, by default FuseBox: 
* flushes all files from *loaded* cache.
* patches the changed files on HMR update.
* runs the application entry point again.

With `setStatefulModules('foo','bar')`: 
* if something other than `foo`, `bar` changed: 
 - flushes all files from *loaded* cache *except* `foo`, and `bar`.
 - patches the changed files on HMR update.
 - runs the application entry point again.
* if `foo`, `bar` changed: 
 - reloads the window.

**Why its useful**:
* For things that register a global hook e.g. `window.addEventListener("hashchange",/*something*/)` you probably don't want these getting flushed from cache and running again.
* For things that initialize / hold state (e.g. MobX stores / Redux Stores / Reflux stores-actions), you probably don't want getting flushed from cache and reinitializing their state.
