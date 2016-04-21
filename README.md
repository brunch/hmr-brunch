# HMR runtime for [Brunch](http://brunch.io)

Allows to use [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement.html) in your Brunch projects.

Constraints:

* Only works for JS files
* Requires brunch v`<unreleased>` and later
* Requires [`auto-reload-brunch`](https://github.com/brunch/auto-reload-brunch) v`<unreleased>` and later
* Provides the main HMR API (but not Management API)
* Works only if your JS compiles to a single file

### Usage

HMR is on by default if the plugin is installed. However, if you need to temporarily disable it, change your config:

```javascript
exports.config = {
  hot: false,
  // ...
};
```

Then, just use the main HMR API:

```javascript
import React from 'react';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import counterApp from './reducers';
import App from 'components/App';

const store = createStore(counterApp, 0);
// detect if we're loading for the first time or reloading
if (module.hot) {
  module.hot.accept('./reducers', (d) => {
    store.replaceReducer(require('./reducers').default);
  });
}
```

Note: in production env, `hmr-brunch` will strip all `if (module.hot) { ... }` conditionals.
