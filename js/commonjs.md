# commonjs

http://www.commonjs.org/

## pain point: relative paths

```js
const utils = require('../../../../../utils')
utils.killMeNow()
```

Relative require's, like above, are counter-productive and unintuitive.

* Typing them is error prone.
* Typing them is time consuming.
* Introspection tooling is a little bit more complex.
  _But there are resolver modules on npm that do this for you..._
* It's different in other languages (& tooled JS eg babel/webpack)

### suggestions

#### deal with it, become disciplined

Unless you want to setup babel/webpack, the next best thing to do is introduce a discipline:

> **If relative requires aren't in the same directory, expand the path to point to the "root" of the app/package.**

**Example Project Structure:**

```
.
├── foo/
│   ├── routes/
│   │   └── route-a.js
│   └── bar/
│       └── baz/
│           ├── down.js
│           └── utils.js
└── index.js
```

`foo/bar/baz/down.js`
```js
// the `utils.js` module is in the same directory, let's require it
const utils = require('./utils')

// the `route-a.js` module is not in the same directory, it's elsewhere
// let's require it from the root of the package
const routeA = require('../../../foo/routes/route-a')
```

#### postinstall link "root"

`package.json`
```json
{
  "scripts": {
    "link-root": "cd node_modules && ln -nsf ../src",
    "postinstall": "npm run link-root"
  }
}
```

`index.js`
```js
const utils = require('src/utils')
```

**Pros**

* Work's like you'd expect it to
* IDE's introspection still works

**Cons**

* postinstall script isn't _always_ run
* IDE's can sometimes break (webstorm seems ok)

#### build src with babel/webpack

Babel & Webpack both support module aliasing.

#### env var NODE_PATH

```shell
NODE_PATH=./src node index.js
```

**Pros**

* nodejs won't deprecate this for a long time

**Cons**

* only good for app's
