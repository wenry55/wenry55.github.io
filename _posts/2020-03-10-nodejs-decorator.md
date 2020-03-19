---
layout: default 
title: Node.JS의 Decorator
categories: [nodejs, decorator]
tags: [nodejs, decorator]
---

# Using Decorator in Node JS

현재, Node JS가 데코레이터를 지원하지는 않습니다. 그래서 이를 사용하기 위해서는 바벨을 이용하여야 합니다. 

먼저 다음과 같은 package들을 설치하시기 바랍니다.
```bash
$ npm install --save-dev babel-cli babel-preset-env 
$ npm install --save-dev babel-plugin-transform-decorators-legacy
```
<br/>

babel을 설치하였으면, 바벨이 사용하는 플러그인을 지정해 주어야 합니다. `.babelrc`에다 다음의 내용을 넣어주세요.
<br/>
##### .babelrc
```json
{
    "plugins": ["transform-decorators-legacy"]
}
```

마지막으로 우리가 사용하는 '@' (데코레이터)를 parsing warning 을 꺼버리기 위해서 jsconfig.json에 다음의 내용을 넣어주세요.

<br/>
##### jsconfig.json
```json
{
    "compilerOptions": {
        "experimentalDecorators": true
    }
}
```

이상의 과정을 마쳤으면 데코레이터를 사용하는 예를 하나 만들어 주세요.

```js
function doSomething(msg) {
    console.log("I'm doing something", msg);
}

function deco(innerFunc) {
    return function() {
        console.log("before innerFunc");
        innerFunc.apply(this, arguments);
        console.log("after innerFunc");
    };
}

function readonly(target, name, descriptor) {
    descriptor.writable = false;
    return descriptor;
}

var decorated = deco(doSomething);

function dec(id){
    console.log('evaluated', id);
    return (target, property, descriptor) => console.log('executed', id);
}

class Example {
    @dec(1)
    @dec(2)
    b() {}
}

var k = new Example();
k.b();
```

이제 마지막으로 package.json의 scripts 영역에 babel-node 를 이용하여 위의 소스를 실행하는 테스트 스크립트를 넣어주세요.

```js
"btest": "babel-node src/test/deco.js"
```

`npm run btest` 를 실행하여 다음과 같은 결과가 나오는지 살펴보세요.

```bash
evaluated 1
evaluated 2
executed 2
executed 1
```


참고) [How to use decorators](https://mobx.js.org/best/decorators.html)

중요한 것 하나 빠졌습니다. 데코레이터 함수는 한번만 eval 됩니다.


Decorators are actually nothing more than functions that return another function, and that are called with the appropriate details of the item being decorated. These decorator functions are evaluated once when the program first runs, and the decorated code is replaced with the return value.