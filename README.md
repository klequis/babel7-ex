# babel7-ex

I was having trouble running code that had been transcompiled vie Babel 7. The issue and solution is document [here](https://stackoverflow.com/questions/54871997/add-is-not-a-function-when-using-babel). In short:
**This doesn't work**
```js
var add = require('./lib/add')

console.log(add(1,3))
```
**Adding `.default` soleves the issue**
```js
var add = require('./lib/add').default

console.log(add(1,3))
```

The answer was found [here](https://stackoverflow.com/questions/33505992/babel-6-changes-how-it-exports-default).

There is a good explanation in [Misunderstanding ES6 Modules, Upgrading Babel, Tears, and a Solution](https://medium.com/@kentcdodds/misunderstanding-es6-modules-upgrading-babel-tears-and-a-solution-ad2d5ab93ce0), and a couple of very good links in there as well.

## Use
```js

```


## The below is already in stackoverflow, but I'll leave it here for reference

**package.json**
```js
{
  "name": "@klequis/npm-step-by-step",
  "version": "0.0.1",
  "description": "learning npm-packaging step by step",
  "main": "index.js",
  "scripts": {
    "build": "babel src --out-dir lib"
  },
  "keywords": [],
  "author": "",
  "license": "MIT",
  "devDependencies": {
    "@babel/cli": "^7.2.3",
    "@babel/core": "^7.3.3",
    "@babel/preset-env": "^7.3.1"
  }
}
```

**src/add.js**
```js
const add = (a, b) => {
  return a + b
}
export default add
```

**lib/add.js - result after running build**
```js
"use strict";

Object.defineProperty(exports, "__esModule", {
  value: true
});
exports.default = void 0;

const add = (a, b) => {
  return a + b;
};

var _default = add;
exports.default = _default;
```

**test.js**
```js
var add = require('./lib/add')

console.log(add(1,3))
```

In test.js, VS Code immediately underlines 'add' with the message: Cannot invoke an expression whose type lacks a call signature. Type 'typeof import("/home/me/dev/learn/npm-packaging/step-by-step/lib/add") has no compatible call signatures.

**run using node on commandlien**
```
$ node test.js
```

**results in **
/home/me/dev/learn/npm-packaging/step-by-step/test.js:3
console.log(add(1,3))
            ^

TypeError: add is not a function
    at Object.<anonymous> (/home/me/dev/learn/npm-packaging/step-by-step/test.js:3:13)
    at Module._compile (internal/modules/cjs/loader.js:689:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:700:10)
    at Module.load (internal/modules/cjs/loader.js:599:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:538:12)
    at Function.Module._load (internal/modules/cjs/loader.js:530:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:742:12)
    at startup (internal/bootstrap/node.js:283:19)
    at bootstrapNodeJSCore (internal/bootstrap/node.js:743:3)