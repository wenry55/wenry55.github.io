---
layout: default
title: one more thing about javascript
categories: [programming, javascript]
tags: [javascript, grammar, argument, destructure]
---

#### Javascript

* argument destructuring

function argument destructure 라고 불리는 이 syntax를 살펴보도록 합시다.

다음과 같은 인자를 받는 함수가 있다고 합시다.

```js
draw_circle({center:[3,4], radius:5})
```

일반적으로 아래와 같이 합니다.

```js
function draw_circle (obj) {
    let x = obj.center[0];
    let y = obj.center[1];
    let r = obj.radius;

    return [x,y,r];

}

console.log (
    draw_circle({center:[3,4], radius:5})
); // [ 3, 4, 5 ]
```

argument destructuring을 이용하면

```js
function draw_circle ({center:[x,y], radius:r}) {
    return [ x, y, r ];
}

console.log (
    draw_circle ({center:[3,4], radius:5})
);
// [ 3, 4, 5 ]
```

