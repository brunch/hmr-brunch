# HMR runtime for [Brunch](http://brunch.io)

Allows to use [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement.html) in your Brunch projects.

Constraints:

* only works for JS files
* requires brunch v`<unreleased>` and later
* requires [`auto-reload-brunch`](https://github.com/brunch/auto-reload-brunch) v`<unreleased>` and later
* provides the main HMR API (but not Management API)

### Usage

Change your config:

```javascript
exports.config = {
  hot: true,
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
