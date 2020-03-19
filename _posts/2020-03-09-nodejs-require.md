---
layout: default 
title: Node.JS의 Require
categories: [nodejs, require]
tags: [nodejs, require]
---

Reference : [Requiring moudles in node js - everything you need to know](https://www.freecodecamp.org/news/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8/)

#  Requiring Modules in Node JS

본 문서에서는 노드가 모듈을 Require하는 방법에 대해서 살펴보겠습니다.

###  Node가 모듈을 찾는 방법 

우리가 다음과 같이 패스를 지정하지 않고 모듈명만 지정하면,

```js
require('find-me')
```

Node는 modules.paths 에 있는 모든 경로를 뒤집니다.

modules.paths는 

```bash
$ node
Welcome to Node.js v12.16.1.
Type ".help" for more information.
> module
Module {
  id: '<repl>',
  path: '.',
  exports: {},
  parent: undefined,
  filename: null,
  loaded: false,
  children: [],
  paths: [
    '/Users/bongkyo/Documents/GitHub/wenry55.github.io/repl/node_modules',
    '/Users/bongkyo/Documents/GitHub/wenry55.github.io/node_modules',
    '/Users/bongkyo/Documents/GitHub/node_modules',
    '/Users/bongkyo/Documents/node_modules',
    '/Users/bongkyo/node_modules',
    '/Users/node_modules',
    '/node_modules',
    '/Users/bongkyo/.node_modules',
    '/Users/bongkyo/.node_libraries',
    '/Users/bongkyo/.nvm/versions/node/v12.16.1/lib/node'
  ]
}
> 
```
위와 같이 확인할 수 있습니다.

위의 경로를 찾다가, 만약 require에 지정된 문자열이 folder를 가리킬 경우, 그 폴더내의 index.js 파일이 사용됩니다. 만약 폴더내의 다른 파일을 로딩해야 할 경우 package.json의 main property를 수정하여 이를 바꿀수도 있습니다. 

### module.exports, 모듈의 중요한 오브젝트 

각 모듈에는 exports 오브젝트가 들어있다.

require 함수를 이용하여 모듈을 로딩할 때, 실제로 리턴되는 것은 `module.exports` 입니다.
즉, `exports` 오브젝트는 Node가 require함수를 이용하여 모듈을 로딩할 때 리턴되는 오브젝트 입니다. 이 때 일반 오브젝트가 아닌 함수오브젝트를 할당하고 이를 리턴할 수도 있습니다.

```bash
$ cat lib/index.js 
exports.a = 1
exports.b = 2
exports['bkseo'] = 3  // exports에 key-value를 할당하듯이 할당할 수 있습니다. 

// module.exports = {} or function() {}

$ cat index.js 
const lib = require('./lib')
console.log("lib", lib)

$ node index.js 
lib { a: 1, b: 2, bkseo: 3 }
```
<br/>
### exports 는 module.exports의 레퍼런스


아래와 같이 exports를 이용해서 exports 오브젝터에 property를 할당할 수 있다. 하지만 두번째 줄의 exports = {...} 와 같은 형태로는 사용이 불가한데, 이는 exports는 module.exports의 레퍼런스 이기 때문이다.

```js
exports.id = 42; // This is ok.
exports = { id: 42 }; // This will not work.
module.exports = { id: 42 }; // This is ok.
```

### 중복로딩 안됨
모듈은 여러번 require 되더라도 한번만 로딩됩니다. 아래를 참조하세요.

```bash
$ cat mymodule.js 
// 모듈이 로딩될 때 아래의 문자열을 출력합니다.
console.log('mymodule.js is loading')

$ cat index.js 
const lib = require('./lib')
// mymodule을 세번 로딩하도록 요청합니다.
require('./mymodule')
require('./mymodule')
require('./mymodule')

console.log("lib", lib)

$ node index.js 
mymodule.js is loading  # 한번만 로딩되는 것을 확인하실 수 있습니다.
lib { a: 1, b: 2, bkseo: 3 }
```
