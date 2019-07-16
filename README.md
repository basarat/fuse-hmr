# WARNING 

[![Greenkeeper badge](https://badges.greenkeeper.io/basarat/fuse-hmr.svg)](https://greenkeeper.io/)

:warning: This package is no longer needed. FuseBox ships with it. [Head here for details!](https://medium.com/@basarat/rethinking-hot-module-reloading-58ce15b5f496#.c04wtx8x6)

# Fuse HMR
A customizable HMR plugin for [FuseBox](http://fuse-box.org/) 🌹

> Built with TypeScript, for TypeScript ❤️️

## Install
```
npm install fuse-box fuse-hmr
```

## Usage

### `setStatefulModules(FuseBox, moduleNames:string[])`

> NOTE: `FuseBox` here refers to the loader API

```
import {setStatefulModules} from 'fuse-hmr';
setStatefulModules(FuseBox,['foo','bar']);
```

On an HMR update, by default FuseBox: 
* flushes all files from *loaded* cache.
* patches the changed files on HMR update.
* runs the application entry point again.

With `setStatefulModules(FuseBox, ['foo','bar'])`: 
* if something other than `foo`, `bar` changed: 
  * flushes all files from *loaded* cache *except* `foo`, and `bar`.
  * patches the changed files on HMR update.
  * runs the application entry point again.
* if `foo`, `bar` changed: 
  * reloads the window.

**Why its useful**:
* For things that register a global hook e.g. `window.addEventListener("hashchange",/*something*/)` you probably don't want these getting flushed from cache and running again.
* For things that initialize / hold state (e.g. MobX stores / Redux Stores / Reflux stores-actions), you probably don't want getting flushed from cache and reinitializing their state.

**Where should you call it**:
In your application entry point e.g.: 

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { App } from "./app";


ReactDOM.render(<App/>, document.getElementById('root'));

import { setStatefulModules } from 'fuse-hmr';
setStatefulModules(FuseBox, ['actions/','stores/', 'router']);
```
