# Import VS Require 


두가지의 모듈시스템.

require?

require는 모듈을 소비한다.

로딩은 싱크로너스하다. 즉, 여러개의 require가 있을때, 하나씩 순차적으로 로딩한다.


What is Import?

Require Throws error at runtime and Import throws error while parsing

우선 ES6 모듈을 지원하는 것은 없음.Babel은 import 와 export 를 CommonJS로 변경함.

named import

![CommonJS VS ES6](../assets/images/commonjs-vs-es6.png)


현재는 ES6 import, export는 항상 CommonJS로 컴파일 된다. 

ES6: `import, export default, export` 
키워드의 개념(단수형)

CommonJS: `require, module.exports, exports.foo`
명령의 개념(exports)


ES6 export default
```js
// hello.js
function hello() {
  return 'hello'
}
export default hello

// app.js
import hello from './hello'
hello() // returns hello


// dep.js
export foo function() {};
export const bar = 22;

// app.js
import {foo, bar} from "dep";
or
import dep from "dep"
dep.foo()
console.log(dep.bar)
```

ES6 export multiple and import multiple
```js
// hello.js
function hello1() {
  return 'hello1'
}
function hello2() {
  return 'hello2'
}
export { hello1, hello2 }

// app.js
import { hello1, hello2 } from './hello'
hello1()  // returns hello1
hello2()  // returns hello2
```

CommonJS module.exports
```js
// hello.js
function hello() {
  return 'hello'
}
module.exports = hello

// app.js
const hello = require('./hello')
hello()   // returns hello
```

CommonJS module.exports multiple

```js
// hello.js
function hello1() {
  return 'hello1'
}
function hello2() {
  return 'hello2'
}
module.exports = {
  hello1,
  hello2
}

// app.js
const hello = require('./hello')
hello.hello1()   // returns hello1
hello.hello2()   // returns hello2
```

